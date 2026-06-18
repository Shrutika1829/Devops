
# Project Title

A brief description of what this project does and who it's for

# 🚀 AWS EC2 Static Website Hosting Using Nginx

Deploying a static website on an AWS EC2 Ubuntu instance using the Nginx web server.

---

## 📋 Project Overview

This project demonstrates how to:

- Launch an AWS EC2 instance
- Connect securely using SSH
- Install and configure Nginx
- Download and deploy a static website template
- Host the website publicly over HTTP

---

## 🛠️ Tech Stack

| Technology | Purpose |
|------------|----------|
| AWS EC2 | Virtual Server |
| Ubuntu Linux | Operating System |
| Nginx | Web Server |
| SSH | Remote Access |
| wget | Download Files |
| unzip | Extract Templates |

---

## 🏗️ Architecture

```text
User Browser
      │
      ▼
AWS EC2 Instance
      │
      ▼
Nginx Web Server
      │
      ▼
Static Website Files
```

---

## 📸 Project Screenshots

### EC2 Instance Running


<img width="1897" height="841" alt="image" src="https://github.com/user-attachments/assets/bbef71ac-92e3-4f61-b4e7-76f56d81f86e" />


---

### SSH Connection Established

<img width="1528" height="842" alt="image" src="https://github.com/user-attachments/assets/32f44063-ca1f-4c53-84ee-646c3ce56d26" />


---

### Nginx Installation

<img width="1883" height="977" alt="image" src="https://github.com/user-attachments/assets/4d1644e9-a170-4eac-9d24-3ef19e656c78" />


---

### Nginx Service Running

<img width="1698" height="802" alt="image" src="https://github.com/user-attachments/assets/f2fc3e6f-07df-4628-a1af-27853b7d90e1" />


---

### Website Successfully Hosted

<img width="1905" height="987" alt="image" src="https://github.com/user-attachments/assets/42793293-2a03-40b7-a35c-0da07038b5c1" />


---

## 🚀 Deployment Steps

### 1️⃣ Connect to EC2 Instance

```bash
ssh -i CBZ-key.pem ubuntu@<PUBLIC-IP>
```

---

### 2️⃣ Update Package Repository

```bash
sudo apt update
sudo apt upgrade -y
```

---

### 3️⃣ Install Nginx

```bash
sudo apt install nginx -y
```

---

### 4️⃣ Verify Nginx Status

```bash
sudo systemctl status nginx
```

---

### 5️⃣ Start and Enable Nginx

```bash
sudo systemctl start nginx
sudo systemctl enable nginx
```

---

### 6️⃣ Install Required Utilities

```bash
sudo apt install wget unzip -y
```

---

### 7️⃣ Download Website Template

```bash
cd /tmp

wget <template-url>
```

---

### 8️⃣ Extract Template

```bash
unzip template.zip
```

---

### 9️⃣ Remove Default Nginx Files

```bash
sudo rm -rf /var/www/html/*
```

---

### 🔟 Deploy Website Files

```bash
sudo cp -r template-folder/* /var/www/html/
```

---

### 1️⃣1️⃣ Restart Nginx

```bash
sudo systemctl restart nginx
```

---

### 1️⃣2️⃣ Access Website

```text
http://<PUBLIC-IP>
```

---

## 📂 Project Structure

```text
aws-ec2-nginx-static-website
│
├── README.md
│
├── screenshots
│   ├── ec2-instance.png
│   ├── ssh-login.png
│   ├── nginx-installation.png
│   ├── nginx-status.png
│   └── website-hosted.png
│
└── commands
    └── deployment-commands.txt
```

---

# Troubleshooting Apache Crashes

This guide outlines how to determine if your Apache web server (`httpd`) has crashed by checking key technical indicators on your server.

---

## Technical Indicators of an Apache Crash

### 1. Active Status Shows "failed" or "inactive"
Run the standard system control status command to check the current state of the service:

```bash
sudo systemctl status httpd
```

* **Signs of a crash:** Instead of `Active: active (running)`, the output will display `Active: failed` or `Active: inactive (dead)` (often highlighted in red text).

---

### 2. Network Ports are Completely Empty
When Apache crashes, it releases its hold on the network ports (typically port 80 and 443). Check if the process is actively listening for incoming traffic:

```bash
sudo ss -tulpn | grep httpd
```

* **Signs of a crash:** The command will return absolutely no output, meaning the web server process is no longer running to accept network connections.

---

### 3. "Exit-code" or "Core Dump" in System Logs
Check the system daemon logs to review recent critical process events:

```bash
sudo journalctl -u httpd --no-pager -n 20
```

* **Signs of a crash:** Look for log lines stating:
  * `Scheduled restart job, restart counter is at X`
  * `Main process exited, code=exited, status=1/FAILURE`
  * `Segmentation fault (core dumped)`

---

### 4. Critical Errors in the Apache Error Log
Open and review the tail end of the primary Apache error log file to find the root cause of the shutdown:

```bash
sudo tail -n 20 /var/log/httpd/error_log
```

* **Signs of a crash:** Look for lines containing high-severity log levels such as `[emerg]` (Emergency), `[crit]` (Critical), or explicit crash signals like `caught SIGSEGV, shutting down`.

---

## 🛠 Next Steps
If you find that Apache has crashed, you can attempt a manual recovery using the following commands:

* **To restart the service:** `sudo systemctl restart httpd`
* **To check configuration syntax errors before starting:** `sudo apachectl configtest`


## 🧠 Key Learnings

- AWS EC2 provisioning
- Linux server administration
- SSH authentication using key pairs
- Nginx installation and configuration
- Static website deployment
- Linux service management using systemctl
- Basic troubleshooting and log analysis

---



.
