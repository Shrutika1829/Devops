
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
# 🚀 Automated EC2 Static Website Deployment

> A seamless, zero-touch solution to provision and launch a production-ready static website on Amazon Web Services (AWS) using **EC2 User Data** automation.

---

### 🎨 Key Architectural Features

* ⚡ **Zero-Touch Execution:** Automatically runs on the very first boot cycle of your EC2 instance.
* 🐧 **Multi-OS Smart Engine:** Intelligently detects and adapts to Ubuntu/Debian (`apt`) or RHEL/CentOS/Rocky Linux (`dnf`/`yum`).
* 🔒 **Permission-Safe Blueprint:** Dynamically secures directories by applying non-root ownership (`www-data` or `nginx`).
* 🛡️ **Firewall Self-Healing:** Autodetects, sets up, and updates system firewalls (`UFW` or `Firewalld`) automatically.

---

## 💻 The Automation Script

Paste this complete code block directly into your AWS EC2 instance configuration panel under **Advanced Details** ➡️ **User Data**.

```bash
#!/bin/bash

# Exit immediately if a command exits with a non-zero status
set -e

# Define variables
DOMAIN="localhost"
WEB_ROOT="/var/www/html"
NGINX_CONF_DIR="/etc/nginx/conf.d"

echo "=== Starting Static Website Deployment ==="

# 1. Install Nginx based on package manager
if [ -x "\$(command -v apt-get)" ]; then
    echo "Detected Debian/Ubuntu system. Installing Nginx..."
    sudo apt-get update
    sudo apt-get install -y nginx
elif [ -x "\$(command -v dnf)" ]; then
    echo "Detected RHEL/CentOS/Fedora system. Installing Nginx..."
    sudo dnf install -y nginx
elif [ -x "\$(command -v yum)" ]; then
    echo "Detected older RHEL/CentOS system. Installing Nginx..."
    sudo yum install -y nginx
else
    echo "Error: Unsupported package manager. Please install Nginx manually."
    exit 1
fi

# 2. Create the web root directory and a sample file
echo "Creating web directory at \$WEB_ROOT..."
sudo mkdir -p "\$WEB_ROOT"

echo "Creating a sample index.html..."
sudo tee "\$WEB_ROOT/index.html" > /dev/null <<EOF
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Welcome to My Static Site</title>
    <style>
        body { font-family: sans-serif; text-align: center; margin-top: 50px; background: #f4f4f9; }
        h1 { color: #333; }
    </style>
</head>
<body>
    <h1>Success! Your static website is live.</h1>
    <p>Deployed automatically via Bash script.</p>
</body>
</html>
EOF

# Set appropriate permissions
echo "Setting permissions..."
if id "nginx" &>/dev/null; then
    sudo chown -R nginx:nginx "\$WEB_ROOT"
elif id "www-data" &>/dev/null; then
    sudo chown -R www-data:www-data "\$WEB_ROOT"
fi
sudo chmod -R 755 "\$WEB_ROOT"

# 3. Configure Nginx Server Block
echo "Configuring Nginx server block..."
sudo mkdir -p "\$NGINX_CONF_DIR"

sudo tee "\$NGINX_CONF_DIR/static_site.conf" > /dev/null <<EOF
server {
    listen 80;
    server_name \$DOMAIN;

    root \$WEB_ROOT;
    index index.html;

    location / {
        try_files \$uri \$uri/ =404;
    }
}
EOF

# 4. Automatically configure the firewall
echo "Checking firewall configuration..."
if [ -x "\$(command -v ufw)" ] && sudo ufw status | grep -q "Status: active"; then
    echo "UFW detected and active. Opening HTTP port..."
    sudo ufw allow 'Nginx HTTP'
elif [ -x "\$(command -v firewall-cmd)" ] && sudo systemctl is-active --quiet firewalld; then
    echo "Firewalld detected and active. Opening HTTP port..."
    sudo firewall-cmd --permanent --add-service=http
    sudo firewall-cmd --reload
else
    echo "No active UFW or Firewalld detected. Skipping firewall rules."
fi

# 5. Start and Enable Nginx
echo "Starting and enabling Nginx service..."
sudo systemctl daemon-reload
sudo systemctl enable nginx
sudo systemctl restart nginx

echo "=== Deployment Complete! ==="
echo "Visit http://\$DOMAIN or your server's IP address to view your site."
```

---

## 🛠️ Step-by-Step AWS Setup Walkthrough

### ⚙️ Step 1: Secure & Configure Network Inbound Traffic
Before launching the server, establish the necessary network inbound rules inside your AWS **Security Group**:

| Rule Type | Port Range | Source Target | Purpose |
| :--- | :--- | :--- | :--- |
| **HTTP** | `80` | `0.0.0.0/0` (Anywhere) | Serves web traffic to the world |
| **SSH** | `22` | `My IP` (Restricted) | Secure CLI system maintenance |

---

### 🖥️ Step 2: Initialize Your Virtual Infrastructure
1. Open the **AWS EC2 Management Console** and click the **Launch Instance** action button.
2. Select your base OS image *(Recommended: **Ubuntu Server** or **Amazon Linux 2023**)*.
3. Choose your instance sizing hardware framework *(e.g., `t2.micro` or `t3.micro` for Free-Tier)*.
4. Attach your preconfigured network **Security Group** crafted in Step 1.

---

### 🪟 Step 3: Inject the Automation Blueprint
1. Head to the bottom block and expand the **Advanced Details** layout configuration.
2. Scroll to the absolute bottom field labeled **User data**.
3. Copy the raw Bash automation code listed above and paste it directly into the open script context text area box.
4. Finalize configurations and hit the **Launch Instance** success trigger.

---

### 🎉 Step 4: Verify Live Status
1. Allow roughly **1 to 2 minutes** for execution pipelines to cleanly finish their boot commands.
2. Extract the public instance address string tracking indicator labeled **Public IPv4 address** inside your active AWS instance window overview panel.
3. Open a clean browser window view and type your server's destination address:
   ```markdown
   http://<YOUR_EC2_PUBLIC_IP_ADDRESS>
   ```

---



.
