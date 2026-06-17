
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

![EC2 Instance](screenshots/ec2-instance.png)

---

### SSH Connection Established

![SSH Login](screenshots/ssh-login.png)

---

### Nginx Installation

![Nginx Installation](screenshots/nginx-installation.png)

---

### Nginx Service Running

![Nginx Status](screenshots/nginx-status.png)

---

### Website Successfully Hosted

![Website](screenshots/website-hosted.png)

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

## 🔍 Verification Commands

### Check Nginx Status

```bash
sudo systemctl status nginx
```

### Verify Port 80

```bash
sudo ss -tulnp | grep nginx
```

### Check Nginx Logs

```bash
sudo tail -f /var/log/nginx/error.log
```

---

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