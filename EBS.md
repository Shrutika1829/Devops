
# 🚀 AWS EBS Hands-on Project

## Create an Amazon EBS Volume, Attach it to an EC2 Instance, Partition, Format, and Mount It

> This project demonstrates how to create an Amazon EBS volume, attach it to an EC2 instance, partition it, format it, mount it, and configure it for automatic mounting after every reboot.

---

# 📖 Project Architecture

```text
                +-----------------------+
                |    AWS EC2 Instance   |
                |   Amazon Linux 2023   |
                +-----------+-----------+
                            |
                     Attach EBS Volume
                            |
                +-----------+-----------+
                |       EBS Volume      |
                |        gp3 (10GB)     |
                +-----------------------+
```

---

# ✅ Prerequisites

* AWS Account
* Running EC2 Instance
* SSH Key Pair (.pem)
* Security Group allowing SSH (Port 22)

---

# Step 1️⃣ Create an EBS Volume

1. Open the **AWS Management Console**
2. Navigate to:

```
EC2
   └── Elastic Block Store
          └── Volumes
```

3. Click **Create Volume**

Use the following settings:

| Property          | Value       |
| ----------------- | ----------- |
| Volume Type       | gp3         |
| Size              | 10 GiB      |
| Availability Zone | Same as EC2 |
| Encryption        | Optional    |

Click **Create Volume**.

---

## ⚠️ Important

The EBS Volume **must** be created in the **same Availability Zone** as your EC2 instance.

✅ Correct

```
EC2 → us-east-1a

EBS → us-east-1a
```

❌ Incorrect

```
EC2 → us-east-1a

EBS → us-east-1b
```

---

# Step 2️⃣ Attach the Volume

Select the volume.

Click

```
Actions
    ↓
Attach Volume
```

Choose

* Running EC2 Instance
* Device Name

Example

```
/dev/xvdf
```

Click **Attach**.

---

# Step 3️⃣ Connect to EC2

```bash
ssh -i key.pem ec2-user@<Public-IP>
```

---

# Step 4️⃣ Verify the New Disk

List available disks.

```bash
lsblk
```

Example

```text
NAME    MAJ:MIN RM SIZE RO TYPE MOUNTPOINT
xvda      202:0   0   8G  0 disk
└─xvda1
xvdf      202:80  0  10G  0 disk
```

Another useful command

```bash
sudo fdisk -l
```

---

# Step 5️⃣ Create a Partition

Start fdisk.

```bash
sudo fdisk /dev/xvdf
```

Inside fdisk

```
n
```

Create a new partition.

```
p
```

Primary partition.

```
1
```

Partition number.

Press **Enter** twice to accept the default values.

Save the changes.

```
w
```

Verify

```bash
lsblk
```

Expected output

```text
xvdf
└── xvdf1
```

---

# Step 6️⃣ Format the Partition

Create an ext4 filesystem.

```bash
sudo mkfs.ext4 /dev/xvdf1
```

---

# Step 7️⃣ Create a Mount Directory

```bash
sudo mkdir /data
```

---

# Step 8️⃣ Mount the Volume

```bash
sudo mount /dev/xvdf1 /data
```

---

# Step 9️⃣ Verify the Mount

```bash
df -h
```

Example

```text
Filesystem      Size Used Avail Mounted on
/dev/xvdf1      10G   24M   10G /data
```

Also verify using

```bash
lsblk
```

Expected

```text
xvdf1
└── /data
```

---

# Step 🔟 Test the Mount

```bash
cd /data

sudo touch test.txt

ls
```

Output

```text
test.txt
```

---

# Step 1️⃣1️⃣ Configure Automatic Mount (Persistent Mount)

Find the UUID.

```bash
sudo blkid
```

Example

```text
/dev/xvdf1: UUID="1234-abcd-5678"
```

Open the fstab file.

```bash
sudo vi /etc/fstab
```

Add the following line.

```text
UUID=1234-abcd-5678   /data   ext4   defaults,nofail   0   2
```

Save the file.

---

# Step 1️⃣2️⃣ Verify fstab

Unmount the filesystem.

```bash
sudo umount /data
```

Mount all filesystems.

```bash
sudo mount -a
```

Verify.

```bash
df -h
```

If there are no errors, the configuration is correct.

---

# Step 1️⃣3️⃣ Test After Reboot

Restart the instance.

```bash
sudo reboot
```

Reconnect to the instance.

Verify the mount.

```bash
df -h
```

The volume should automatically mount at

```
/data
```

---

# 📌 Useful Commands

### List disks

```bash
lsblk
```

### Show mounted filesystems

```bash
df -h
```

### Display partition table

```bash
sudo fdisk -l
```

### Display filesystem UUID

```bash
sudo blkid
```

### Check filesystem type

```bash
lsblk -f
```

### Mount manually

```bash
sudo mount /dev/xvdf1 /data
```

### Unmount

```bash
sudo umount /data
```

---

# 🧹 Cleanup

Unmount the volume.

```bash
sudo umount /data
```

Detach the volume.

```
EC2 Console
→ Volumes
→ Select Volume
→ Actions
→ Detach Volume
```

Delete the volume if it is no longer required to avoid additional AWS charges.

---

# 💡 Interview Questions

### Why do we partition an EBS volume?

Partitioning divides a disk into logical sections, allowing separate filesystems and easier storage management.

---

### What is the difference between formatting and mounting?

* **Formatting** creates a filesystem (such as ext4 or xfs) on the partition.
* **Mounting** makes the filesystem accessible through a directory.

---

### Why is `/etc/fstab` used?

It ensures the filesystem is automatically mounted every time the EC2 instance boots.

---

### How do you verify whether an EBS volume is mounted?

```bash
df -h
```

or

```bash
lsblk
```

---

### Can an EBS volume be attached to multiple EC2 instances?

Only **io1** and **io2** volumes support **Multi-Attach**, and only on supported Nitro-based instances.

---

# 🎯 Learning Outcomes

After completing this project, you will be able to:

* ✅ Create an Amazon EBS volume
* ✅ Attach an EBS volume to an EC2 instance
* ✅ Create Linux partitions
* ✅ Format a partition with a filesystem
* ✅ Mount an EBS volume
* ✅ Configure automatic mounting using `/etc/fstab`
* ✅ Verify storage using Linux commands
* ✅ Understand common AWS EBS interview concepts

