# 🐧 Linux Commands Documentation

## 📖 Introduction

This repository contains commonly used Linux commands for system administration, user management, process management, file handling, permissions, networking, and troubleshooting.

---

# 📂 File and Directory Management

## Check Current Directory

```bash
pwd
```

Displays the present working directory.

### Example

```bash
ubuntu@server:~$ pwd
/home/ubuntu
```

---

## List Files and Directories

```bash
ls
```

### Common Options

```bash
ls -l
ls -a
ls -lh
```

| Command | Description |
|----------|-------------|
| ls | List files |
| ls -l | Long listing format |
| ls -a | Show hidden files |
| ls -lh | Human-readable sizes |

---

## Create Directory

```bash
mkdir project
```

Create multiple directories:

```bash
mkdir dir1 dir2 dir3
```

---

## Change Directory

```bash
cd directory_name
```

Examples:

```bash
cd /etc
cd ~
cd ..
```

---

## Remove Directory

```bash
rmdir directory_name
```

Remove non-empty directory:

```bash
rm -r directory_name
```

---

# 📄 File Operations

## Create Empty File

```bash
touch file.txt
```

---

## View File Content

```bash
cat file.txt
```

Other commands:

```bash
less file.txt
more file.txt
```

---

## Copy Files

```bash
cp source.txt destination.txt
```

Copy directory:

```bash
cp -r source_dir destination_dir
```

---

## Move/Rename Files

```bash
mv oldname.txt newname.txt
```

Move file:

```bash
mv file.txt /tmp/
```

---

## Delete Files

```bash
rm file.txt
```

Force delete:

```bash
rm -f file.txt
```

Delete directory recursively:

```bash
rm -rf directory_name
```

---

# 👤 User Management

## Create User

```bash
sudo useradd username
```

Create user with home directory:

```bash
sudo useradd -m username
```

---

## Set Password

```bash
sudo passwd username
```

---

## Switch User

```bash
su username
```

Login shell:

```bash
su - username
```

---

## Delete User

Delete only user:

```bash
sudo userdel username
```

Delete user with home directory:

```bash
sudo userdel -r username
```

---

## Check User Information

```bash
id username
```

```bash
whoami
```

```bash
who
```

---

# 👥 Group Management

## Create Group

```bash
sudo groupadd devops
```

---

## Add User to Group

```bash
sudo usermod -aG devops username
```

---

## Remove User from Group

```bash
sudo gpasswd -d username devops
```

---

## View Groups

```bash
groups username
```

---

# 🔐 Permission Management

## View Permissions

```bash
ls -l
```

Example:

```bash
-rw-r--r-- 1 ubuntu ubuntu 100 Jul 15 file.txt
```

---

## Change Permissions

```bash
chmod 755 file.sh
```

Common Permissions:

| Permission | Meaning |
|------------|----------|
| 777 | Full Access |
| 755 | Owner RWX, Others RX |
| 644 | Owner RW, Others R |
| 600 | Owner RW Only |

Examples:

```bash
chmod 644 file.txt
chmod +x script.sh
```

---

## Change Ownership

```bash
sudo chown user file.txt
```

Change owner and group:

```bash
sudo chown user:group file.txt
```

---

# ⚙️ Process Management

## View Running Processes

```bash
ps -ef
```

```bash
ps aux
```

---

## Real-Time Process Monitoring

```bash
top
```

Alternative:

```bash
htop
```

---

## Find Process

```bash
pidof nginx
```

```bash
pgrep nginx
```

---

## Kill Process

```bash
kill PID
```

Force kill:

```bash
kill -9 PID
```

Kill by process name:

```bash
pkill nginx
```

---

# 💾 Disk Management

## Check Disk Usage

```bash
df -h
```

---

## Check Directory Size

```bash
du -sh directory_name
```

---

## List Block Devices

```bash
lsblk
```

---

# 🌐 Networking Commands

## Check IP Address

```bash
ip a
```

---

## Test Connectivity

```bash
ping google.com
```

---

## Check Open Ports

```bash
ss -tulnp
```

Alternative:

```bash
netstat -tulnp
```

---

## DNS Lookup

```bash
nslookup google.com
```

---

## Download Files

```bash
wget https://example.com/file.zip
```

```bash
curl -O https://example.com/file.zip
```

---

# 📦 Package Management (Ubuntu)

## Update Packages

```bash
sudo apt update
```

---

## Upgrade Packages

```bash
sudo apt upgrade -y
```

---

## Install Package

```bash
sudo apt install nginx -y
```

---

## Remove Package

```bash
sudo apt remove nginx -y
```

---

# 🗜️ Archive and Compression

## Create Tar Archive

```bash
tar -cvf backup.tar folder/
```

---

## Extract Tar Archive

```bash
tar -xvf backup.tar
```

---

## Create Compressed Archive

```bash
tar -czvf backup.tar.gz folder/
```

---

## Extract Compressed Archive

```bash
tar -xzvf backup.tar.gz
```

---

# 📊 System Monitoring

## Check Memory Usage

```bash
free -h
```

---

## Check CPU Information

```bash
lscpu
```

---

## Check Uptime

```bash
uptime
```

---

## View System Logs

```bash
journalctl
```

Latest logs:

```bash
journalctl -xe
```
# 🔍 Text Processing Commands

## grep - Search Text Patterns

Search for a specific word inside a file.

```bash
grep "error" logfile.txt
```

Ignore case:

```bash
grep -i "error" logfile.txt
```

Show line numbers:

```bash
grep -n "error" logfile.txt
```

Search recursively in directories:

```bash
grep -r "error" /var/log/
```

---

## sed - Stream Editor

Replace text in output:

```bash
sed 's/nginx/apache/' file.txt
```

Replace all occurrences:

```bash
sed 's/nginx/apache/g' file.txt
```

Modify file directly:

```bash
sed -i 's/nginx/apache/g' file.txt
```

Delete a specific line:

```bash
sed '3d' file.txt
```

Print specific line:

```bash
sed -n '5p' file.txt
```

---

## awk - Pattern Scanning and Processing

Print first column:

```bash
awk '{print $1}' file.txt
```

Print first and third columns:

```bash
awk '{print $1,$3}' file.txt
```

Display usernames from passwd file:

```bash
awk -F ":" '{print $1}' /etc/passwd
```

Show username and shell:

```bash
awk -F ":" '{print $1,$7}' /etc/passwd
```

Count lines:

```bash
awk 'END {print NR}' file.txt
```

---

# 🔎 File Searching

## find Command

Find file by name:

```bash
find /home -name file.txt
```

Find directories:

```bash
find /home -type d
```

Find files:

```bash
find /home -type f
```

Find files larger than 100 MB:

```bash
find / -size +100M
```

Find files modified in last 7 days:

```bash
find /home -mtime -7
```

Find and delete files:

```bash
find /tmp -name "*.log" -delete
```

---

# ⚙️ Process Management

## ps aux

Display all running processes.

```bash
ps aux
```

### Columns

| Column | Meaning |
|----------|----------|
| USER | Process owner |
| PID | Process ID |
| %CPU | CPU usage |
| %MEM | Memory usage |
| COMMAND | Executed command |

Example:

```bash
ps aux | grep nginx
```

---

## ps -elf

Detailed process information.

```bash
ps -elf
```

Useful for viewing:

- Parent Process ID (PPID)
- Process State
- Priority
- Complete Command

Example:

```bash
ps -elf | grep sshd
```

---

## Kill Commands

### Graceful Termination (SIGTERM)

```bash
kill -15 PID
```

or

```bash
kill PID
```

Signal 15 asks the process to terminate safely.

Example:

```bash
kill -15 1234
```

---

### Pause/Suspend Process (SIGSTOP)

```bash
kill -19 PID
```

Stops a process temporarily without terminating it.

Example:

```bash
kill -19 1234
```

Verify:

```bash
ps -elf | grep 1234
```

State may show:

```bash
T
```

(T = Stopped)

Resume process:

```bash
kill -18 PID
```

---

### Force Kill Process (SIGKILL)

```bash
kill -9 PID
```

Immediately terminates the process.

Example:

```bash
kill -9 1234
```

---

# 👤 User Password Aging

## chage Command

Manage password aging policies.

View password aging details:

```bash
chage -l username
```

Example Output:

```text
Last password change
Password expires
Password inactive
Account expires
```

---

Set password expiry after 90 days:

```bash
sudo chage -M 90 username
```

Set minimum days before password change:

```bash
sudo chage -m 7 username
```

Force password change on next login:

```bash
sudo chage -d 0 username
```

Set account expiry date:

```bash
sudo chage -E 2026-12-31 username
```

---

# 🎯 Common Interview Examples

### Find files larger than 500 MB

```bash
find / -size +500M
```

### Display all usernames

```bash
awk -F ":" '{print $1}' /etc/passwd
```

### Replace word in file

```bash
sed -i 's/dev/test/g' file.txt
```

### Search for failed login attempts

```bash
grep "Failed" /var/log/auth.log
```

### Stop a process temporarily

```bash
kill -19 PID
```

### Resume stopped process

```bash
kill -18 PID
```

### Gracefully stop process

```bash
kill -15 PID
```

### Check running nginx process

```bash
ps aux | grep nginx
```

### View detailed process hierarchy

```bash
ps -elf
```
---


⭐ If you found this repository useful, don't forget to star it.
