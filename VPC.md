# 🌐 AWS VPC Lab — Complete Step-by-Step Guide
---

## 📋 Table of Contents

1. [Architecture Overview](#architecture-overview)
2. [Phase 1 — Create the VPC](#phase-1--create-the-vpc)
3. [Phase 2 — Create Public and Private Subnets](#phase-2--create-public-and-private-subnets)
4. [Phase 3 — Create and Attach an Internet Gateway](#phase-3--create-and-attach-an-internet-gateway)
5. [Phase 4 — Configure the Public Route Table](#phase-4--configure-the-public-route-table)
6. [Phase 5 — Launch EC2 Instances](#phase-5--launch-ec2-instances)
7. [Phase 6 — Connect to Public Instance and Test Internet](#phase-6--connect-to-public-instance-and-test-internet)
8. [Phase 7 — Create a NAT Gateway](#phase-7--create-a-nat-gateway)
9. [Phase 8 — Create Private Route Table](#phase-8--create-private-route-table)
10. [Phase 9 — SSH into Private Instance via Bastion](#phase-9--ssh-into-private-instance-via-bastion)
11. [Troubleshooting](#troubleshooting)
12. [Cleanup](#cleanup)

---

## Architecture Overview

```
┌─────────────────────────────────── AWS ─────────────────────────────────────┐
│                              myvpc  10.0.0.0/16                              │
│                                                                              │
│   ┌────────────────────────────────┐   ┌──────────────────────────────────┐ │
│   │   Public Subnet  10.0.1.0/24  │   │  Private Subnet  10.0.2.0/24    │ │
│   │                                │   │                                  │ │
│   │   ┌────────────────────────┐   │   │   ┌────────────────────────┐    │ │
│   │   │   Bastion EC2          │   │   │   │   Private EC2          │    │ │
│   │   │   (public-bastion)     │   │   │   │   (private-app-server) │    │ │
│   │   │   Public IP: x.x.x.x  │───┼───┼──▶│   Private IP:10.0.2.x  │    │ │
│   │   │   Private IP:10.0.1.x  │   │   │   │   No Public IP         │    │ │
│   │   └────────────────────────┘   │   │   └────────────────────────┘    │ │
│   │                                │   │             ▲                    │ │
│   │   ┌────────────────────────┐   │   │   Internet via NAT GW            │ │
│   │   │   NAT Gateway          │───┼───┘                                  │ │
│   │   │   (Elastic IP)         │   │                                      │ │
│   │   └───────────┬────────────┘   │                                      │ │
│   └───────────────┼────────────────┘   └──────────────────────────────────┘ │
│                   │                                                          │
│   ┌───────────────▼────────────────┐                                        │
│   │   Internet Gateway (igw-myvpc) │                                        │
│   └───────────────┬────────────────┘                                        │
└───────────────────┼─────────────────────────────────────────────────────────┘
                    │
                 INTERNET
```

### Resource Summary

| Component | Name | Value |
|-----------|------|-------|
| VPC | myvpc | `10.0.0.0/16` |
| Public Subnet | public-subnet | `10.0.1.0/24` |
| Private Subnet | private-subnet | `10.0.2.0/24` |
| Internet Gateway | igw-myvpc | Attached to myvpc |
| Public Route Table | public-rt | `0.0.0.0/0 → IGW` |
| Private Route Table | private-rt | `0.0.0.0/0 → NAT GW` |
| NAT Gateway | nat-gw-public | Deployed in public-subnet |
| Bastion EC2 | public-bastion | Has public IP |
| App Server EC2 | private-app-server | No public IP |

---

## Phase 1 — Create the VPC

A **Virtual Private Cloud (VPC)** is your isolated network inside AWS. Every resource in this lab lives inside `myvpc`.

### Steps

1. Open **AWS Console → Services → VPC**
2. In the left panel, click **Your VPCs**
3. Click **Create VPC**
4. Fill in the form:

   | Field | Value |
   |-------|-------|
   | Name tag | `myvpc` |
   | IPv4 CIDR block | `10.0.0.0/16` |
   | Tenancy | Default |

5. Click **Create VPC**

> **Note:** The `/16` CIDR gives you 65,536 IP addresses to split across subnets.

---

## Phase 2 — Create Public and Private Subnets

Subnets are IP ranges inside your VPC. The public subnet will have a route to the internet; the private subnet will not.

### 2A — Create the Public Subnet

1. Go to **VPC Console → Subnets → Create subnet**
2. **VPC ID** → Select `myvpc`
3. Fill in:

   | Field | Value |
   |-------|-------|
   | Subnet name | `public-subnet` |
   | Availability Zone | e.g. `us-east-1a` |
   | IPv4 CIDR block | `10.0.1.0/24` |

4. Click **Create subnet**

### 2B — Create the Private Subnet

1. Click **Create subnet** again
2. **VPC ID** → Select `myvpc`
3. Fill in:

   | Field | Value |
   |-------|-------|
   | Subnet name | `private-subnet` |
   | Availability Zone | Same or different AZ |
   | IPv4 CIDR block | `10.0.2.0/24` |

4. Click **Create subnet**

### 2C — Enable Auto-Assign Public IPv4 on the Public Subnet

By default, EC2 instances do **not** get a public IP. We enable auto-assignment on the public subnet so instances can be reached from the internet.

1. In **Subnets**, select `public-subnet`
2. Click **Actions → Edit subnet settings**
3. Check **Enable auto-assign public IPv4 address**
4. Click **Save**

> **Note:** Do NOT enable this on the private subnet. Private instances should never have direct internet access.

---

## Phase 3 — Create and Attach an Internet Gateway

An **Internet Gateway (IGW)** is the AWS-managed gateway between your VPC and the public internet. Without it, nothing in your VPC can reach the internet — even if an instance has a public IP.

### 3A — Create the Internet Gateway

1. Go to **VPC Console → Internet Gateways → Create internet gateway**

   | Field | Value |
   |-------|-------|
   | Name tag | `igw-myvpc` |

2. Click **Create internet gateway**

### 3B — Attach the IGW to myvpc

1. After creation, the IGW will show state **Detached**
2. Click **Actions → Attach to VPC**
3. Select `myvpc` from the dropdown
4. Click **Attach internet gateway**

> **Note:** Each VPC can only have **ONE** Internet Gateway attached. The IGW is highly available and scales automatically — you never manage it.

---

## Phase 4 — Configure the Public Route Table

A **route table** contains rules that determine where network traffic goes. We need to add a route pointing to the IGW so the public subnet can reach the internet.

### 4A — Add the Internet Route

1. Go to **VPC Console → Route Tables**
2. Find the route table associated with `myvpc` (name it `public-rt` for clarity)
3. Click on it → go to the **Routes** tab
4. Click **Edit routes → Add route**

   | Field | Value |
   |-------|-------|
   | Destination | `0.0.0.0/0` |
   | Target | Internet Gateway → `igw-myvpc` |

5. Click **Save changes**

### 4B — Associate Public Subnet

1. Go to the **Subnet associations** tab on `public-rt`
2. Click **Edit subnet associations**
3. Check `public-subnet`
4. Click **Save associations**

> **Note:** The route `0.0.0.0/0 → IGW` means: for any traffic going outside the VPC, send it to the Internet Gateway. This is what makes a subnet **public**.

---

## Phase 5 — Launch EC2 Instances

We launch two EC2 instances: a **bastion host** in the public subnet and an **app server** in the private subnet.

> ⚠️ **Critical:** You MUST change the VPC and Subnet under Network settings when launching each instance. If you forget, the instance launches into the default VPC.

### 5A — Launch Public EC2 (Bastion Host)

1. Go to **EC2 Console → Instances → Launch instances**
2. Fill in:

   | Setting | Value |
   |---------|-------|
   | Name | `public-bastion` |
   | AMI | Ubuntu Server 22.04 LTS (Free Tier eligible) |
   | Instance type | `t2.micro` |
   | Key pair | Create new → `lab-key` → **Download .pem file and save it safely** |

3. Under **Network settings → click Edit**:

   | Field | Value |
   |-------|-------|
   | VPC | `myvpc` |
   | Subnet | `public-subnet` |
   | Auto-assign public IP | Enable |

4. **Security Group** → Create new `public-sg`:
   - Inbound: SSH (port 22) from your IP or `0.0.0.0/0` (lab only)

5. Click **Launch instance**

### 5B — Launch Private EC2 (App Server)

1. Go to **EC2 Console → Instances → Launch instances**
2. Fill in:

   | Setting | Value |
   |---------|-------|
   | Name | `private-app-server` |
   | AMI | Ubuntu Server 22.04 LTS |
   | Instance type | `t2.micro` |
   | Key pair | Same key pair → `lab-key` |

3. Under **Network settings → click Edit**:

   | Field | Value |
   |-------|-------|
   | VPC | `myvpc` |
   | Subnet | `private-subnet` |
   | Auto-assign public IP | Disable |

4. **Security Group** → Create new `private-sg`:
   - Inbound: SSH (port 22) from `10.0.1.0/24` (public subnet) or from `public-sg`

5. Click **Launch instance**

> **Note:** The private instance will have **no public IP**. It is only reachable via the bastion host inside the VPC.

---

## Phase 6 — Connect to Public Instance and Test Internet

### 6A — SSH from Your Local Machine

1. Note the **Public IPv4 address** of `public-bastion` from the EC2 console
2. Open your terminal (Linux/Mac) or PuTTY/MobaXterm (Windows)
3. Set correct permissions on your key file:

   ```bash
   chmod 400 lab-key.pem
   ```

4. SSH into the bastion:

   ```bash
   ssh -i "lab-key.pem" ubuntu@<PUBLIC_IP_OF_BASTION>
   ```

5. Type `yes` when prompted about the host fingerprint

### 6B — Test Internet Connectivity

Once logged into the bastion, run:

```bash
# Test ICMP (ping)
ping google.com -c 4

# Confirm your public IP (outbound internet works)
curl -s https://ifconfig.me
```

You should see replies — internet is working via the IGW. ✅

> **Note:** If ping fails, check the route table has `0.0.0.0/0 → IGW` and the security group allows outbound traffic (default SGs allow all outbound).

---

## Phase 7 — Create a NAT Gateway

The private subnet has no internet access by default. A **NAT Gateway** lets private instances make outbound connections (e.g. to download packages) WITHOUT being directly reachable from the internet.

> ⚠️ **Critical:** The NAT Gateway must be deployed in the **PUBLIC subnet**, not the private one. It needs access to the IGW to route traffic.

### 7A — Create the NAT Gateway

1. Go to **VPC Console → NAT Gateways → Create NAT gateway**

   | Field | Value |
   |-------|-------|
   | Name | `nat-gw-public` |
   | Subnet | `public-subnet` ← **must be public subnet** |
   | Connectivity type | Public |

2. Click **Allocate Elastic IP** — this assigns a static public IP to the NAT GW
3. Click **Create NAT gateway**
4. **Wait 1–2 minutes** until the status changes from `Pending` → `Available`

> **Note:** Elastic IPs (EIPs) are static public IPs. The NAT Gateway uses this EIP to communicate with the internet on behalf of your private instances.

---

## Phase 8 — Create Private Route Table

We need a **separate route table** for the private subnet. This table sends internet-bound traffic to the NAT Gateway (not the IGW directly).

### 8A — Create the Route Table

1. Go to **VPC Console → Route Tables → Create route table**

   | Field | Value |
   |-------|-------|
   | Name | `private-rt` |
   | VPC | `myvpc` |

2. Click **Create route table**

### 8B — Add NAT Gateway Route

1. Select `private-rt` → go to **Routes** tab
2. Click **Edit routes → Add route**

   | Field | Value |
   |-------|-------|
   | Destination | `0.0.0.0/0` |
   | Target | NAT Gateway → `nat-gw-public` |

3. Click **Save changes**

### 8C — Associate Private Subnet

1. Go to **Subnet associations** tab on `private-rt`
2. Click **Edit subnet associations**
3. Check `private-subnet`
4. Click **Save associations**

> **Note:** Traffic flow for private instances:
> `Private EC2 → NAT GW (public subnet) → IGW → Internet`
> The private instance never gets a public IP but can still reach the internet outbound.

---

## Phase 9 — SSH into Private Instance via Bastion

Since the private instance has **no public IP**, you cannot SSH into it directly. You must first SSH into the bastion, then hop from the bastion into the private instance. This is the **bastion host / jump server** pattern.

> ⚠️ **Important:** The key pair file (`lab-key.pem`) must be present on the bastion host to authenticate to the private instance.

### 9A — Copy Your Key Pair to the Bastion Host

From your **local machine terminal** (NOT inside any SSH session), run:

```bash
scp -i "lab-key.pem" lab-key.pem ubuntu@<PUBLIC_IP_OF_BASTION>:~
```

This copies `lab-key.pem` to the home directory (`~`) of the bastion.

### 9B — SSH into the Bastion Host

```bash
ssh -i "lab-key.pem" ubuntu@<PUBLIC_IP_OF_BASTION>
```

### 9C — Set Permissions on the Key File (on the Bastion)

Once inside the bastion terminal, run:

```bash
chmod 400 lab-key.pem
```

> SSH will **refuse** to use a key file with permissions too open (e.g. 644). This step is mandatory.

### 9D — Get the Private IP of the Private Instance

1. Go to **EC2 Console → Instances → Select `private-app-server`**
2. Note the **Private IPv4 address** (e.g. `10.0.2.45`)

### 9E — SSH from Bastion into the Private Instance

From **inside the bastion terminal**, run:

```bash
ssh -i "lab-key.pem" ubuntu@<PRIVATE_IP_OF_PRIVATE_EC2>
```

**Example:**

```bash
ssh -i "lab-key.pem" ubuntu@10.0.2.45
```

Type `yes` when prompted. You are now **inside the private instance**. ✅

### 9F — Verify Internet Access on the Private Instance

```bash
# Test outbound internet via NAT Gateway
ping google.com -c 4

# Download packages (should work)
sudo apt update
```

Both should succeed — proving the private instance can reach the internet via NAT GW, without having a public IP. ✅

> **Note:** Even though the private instance has outbound internet access, **no one from the internet can initiate a connection to it**. This is the security benefit of NAT.

---

## Troubleshooting

| Problem | Fix |
|---------|-----|
| Cannot SSH to bastion | Check SG allows port 22 from your IP. Verify public IP is assigned to the instance. |
| Bastion has no internet | Check `public-rt` has `0.0.0.0/0 → IGW` and `public-subnet` is associated. |
| SSH permission denied (key) | Run `chmod 400 lab-key.pem` — permissions must be restricted to owner only. |
| Cannot SSH from bastion to private instance | Ensure `lab-key.pem` is on the bastion. `private-sg` must allow port 22 from `10.0.1.0/24`. |
| Private instance has no internet | Check `private-rt` has `0.0.0.0/0 → NAT GW`. NAT GW must be in `Available` state. |
| NAT GW still shows `Pending` | Wait 1–2 more minutes. NAT Gateway provisioning takes time. |
| EC2 launched in wrong VPC/subnet | Terminate instance, re-launch and explicitly select `myvpc` and the correct subnet. |

---

## Cleanup (Avoid Charges)

Delete resources in this exact order to avoid errors and unnecessary charges:

1. **Terminate** both EC2 instances (`public-bastion`, `private-app-server`)
2. **Delete** the NAT Gateway — it charges per hour + per GB of data
3. **Release** the Elastic IP → Actions → Release Elastic IP address
4. **Detach and Delete** the Internet Gateway (`igw-myvpc`)
5. **Delete** the private route table (`private-rt`)
6. **Delete** subnets (`public-subnet`, `private-subnet`)
7. **Delete** the VPC (`myvpc`)

> ⚠️ **NAT Gateways** are the most expensive resource in this lab (~$0.045/hour + data charges). Always delete them first when done.

---

