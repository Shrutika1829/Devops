
🚀 Amazon EFS with EC2


Create and Mount an Amazon Elastic File System (EFS) on an EC2 Instance


This project covers:

- Creating an Amazon EFS File System
- Configuring Security Groups
- Creating Mount Targets
- Installing Amazon EFS Utilities
- Mounting EFS on EC2
- Verifying Shared Storage
- Configuring Persistent Mounts
- Troubleshooting Common Issues

---

# 🏗️ Architecture



```
                  +---------------------------+
                  |      Amazon EFS           |
                  |  Shared Network Storage   |
                  +------------+--------------+
                               |
                         NFS Port 2049
                               |
         ---------------------------------------------
         |                                           |
+----------------------+                 +----------------------+
| EC2 Instance (1a)    |                 | EC2 Instance (1c)    |
| Mount Point: /efs    |                 | Mount Point: /efs    |
+----------------------+                 +----------------------+

         Both instances access the same shared files
```


---

# ☁️ AWS Services Used

| Service | Purpose |
|----------|---------|
| Amazon EC2 | Virtual Server |
| Amazon EFS | Shared File System |
| VPC | Network Isolation |
| Security Group | Firewall |
| Subnet | Network Segmentation |

---

# 📋 Prerequisites

Before starting, make sure you have:

- AWS Account
- Amazon Linux 2 EC2 Instance
- Key Pair (.pem)
- Internet Connectivity
- Security Group
- Same VPC for EC2 and EFS

---

# 🚀 Step 1 - Launch an EC2 Instance

Launch an Amazon Linux 2 EC2 instance.

Configuration:

- Amazon Linux 2
- t2.micro
- Same VPC
- Public IP Enabled
- Attach Security Group

Connect via SSH

```bash
ssh -i my-key.pem ec2-user@<Public-IP>
```

---

# 🚀 Step 2 - Create Amazon EFS

Navigate to

```
AWS Console
→ Amazon EFS
→ Create File System
```

Choose

- Same VPC
- Regional
- General Purpose
- Bursting Throughput

Click **Create**.

---

# 🚀 Step 3 - Configure Security Groups

## EC2 Security Group

| Type | Port | Source |
|------|------|---------|
| SSH | 22 | My IP |
| NFS | 2049 | EFS Security Group |

---

## EFS Security Group

| Type | Port | Source |
|------|------|---------|
| NFS | 2049 | EC2 Security Group |

---

# 🚀 Step 4 - Install Amazon EFS Utilities

Update packages

```bash
sudo yum update -y
```

Install EFS utilities

```bash
sudo yum install amazon-efs-utils -y
```

Verify installation

```bash
rpm -qa | grep efs
```

---

# 🚀 Step 5 - Create Mount Directory

```bash
sudo mkdir /efs
```

Verify

```bash
ls /
```

---

# 🚀 Step 6 - Get EFS DNS Name

Go to

```
EFS
→ Select File System
→ Attach
```

Copy the DNS name.

Example

```
fs-xxxxxxxx.efs.us-east-1.amazonaws.com
```

---

# 🚀 Step 7 - Mount EFS

```bash
sudo mount -t efs fs-xxxxxxxx:/ /efs
```

Alternative (NFS)

```bash
sudo mount -t nfs4 \
-o nfsvers=4.1 \
fs-xxxxxxxx.efs.us-east-1.amazonaws.com:/ /efs
```

---

# 🚀 Step 8 - Verify Mount

```bash
df -h
```

Example Output

```
Filesystem                  Size Used Avail Mounted on
127.0.0.1:/                 8.0E    0   8.0E    /efs
```

Check mounted filesystem

```bash
mount | grep efs
```

---

# 🚀 Step 9 - Test Shared Storage

Navigate

```bash
cd /efs
```

Create a file

```bash
touch demo.txt
```

Write data

```bash
echo "Hello from EC2 Instance 1" > demo.txt
```

Verify

```bash
cat demo.txt
```

---

# 🚀 Step 10 - Mount on Second EC2

Repeat

- Install amazon-efs-utils
- Create /efs directory
- Mount same EFS

```bash
sudo mount -t efs fs-xxxxxxxx:/ /efs
```

Verify

```bash
cat /efs/demo.txt
```

Expected Output

```
Hello from EC2 Instance 1
```

---

# 💾 Configure Persistent Mount

Edit

```bash
sudo vi /etc/fstab
```

Add

```text
fs-xxxxxxxx:/ /efs efs defaults,_netdev 0 0
```

Test

```bash
sudo mount -a
```

Verify

```bash
df -h
```

---

# 📸 Screenshots

Add screenshots inside a folder named **screenshots**

```
screenshots/
│
├── create-efs.png
├── mount-target.png
├── security-group.png
├── install-utils.png
├── mount-success.png
├── df-output.png
├── shared-storage.png
└── persistent-mount.png
```

Example

```markdown
## Create EFS

![Create EFS](screenshots/create-efs.png)

## Successful Mount

![Mount Success](screenshots/mount-success.png)
```

---

# 🛠️ Useful Commands

Check mounted filesystem

```bash
df -h
```

List mounts

```bash
mount
```

Check EFS mount

```bash
mount | grep efs
```

Disk usage

```bash
du -sh /efs
```

Unmount

```bash
sudo umount /efs
```

---

# ❗ Troubleshooting

| Issue | Solution |
|--------|----------|
| Mount Timeout | Verify Port 2049 |
| Connection Refused | Check Mount Target |
| DNS Error | Enable DNS Hostnames |
| Permission Denied | Check Ownership |
| Mount Failed | Verify EFS Utilities |

---

# 🌍 Real-World Use Cases

- WordPress Shared Storage
- Shared Web Servers
- Machine Learning Data
- Shared Application Logs
- CI/CD Artifact Storage
- Kubernetes Persistent Storage
- Multi-AZ Applications

---

# 🎯 Learning Outcomes

After completing this project, you will understand:

- Amazon EFS Architecture
- Difference between EFS and EBS
- Security Groups
- Mount Targets
- Shared File Storage
- Linux Mount Commands
- Persistent Mounts
- EFS Troubleshooting

---

