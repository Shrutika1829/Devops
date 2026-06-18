
# EC2 Services
---

## 📋 Prerequisites
Before you begin, ensure you have:
* An active **AWS Account** with administrative or EC2 creation permissions.
* A **web browser** logged into the [AWS Management Console](https://amazon.com).

---

## 🚀 Phase 1: Launching an EC2 Instance

Follow these steps to deploy a new virtual machine.

### Step 1: Navigate to EC2
1. Log in to your **AWS Management Console**.
2. Type **EC2** into the global search bar at the top of the screen.
3. Click on **EC2** from the services list.
4. Check the **Region** dropdown in the upper right corner (e.g., *N. Virginia*, *Oregon*) to ensure you are in your preferred location.

### Step 2: Initialize Configuration
1. On the main EC2 Dashboard, click the orange **Launch instance** button.

### Step 3: Base Configurations
1. **Name and tags:** Enter a recognizable name in the text box (e.g., `production-web-server`).
2. **Application and OS Images (AMI):** 
   * Select your preferred operating system (e.g., *Amazon Linux 2023* or *Ubuntu*).
   * **Tip:** Choose an image explicitly marked **Free tier eligible** to avoid accidental infrastructure costs.
3. **Instance type:** Select your hardware size. 
   * Choose `t2.micro` or `t3.micro` to remain within the AWS Free Tier.

### Step 4: Access & Networking Security
1. **Key pair (login):** 
   * Select an existing key pair, or click **Create new key pair**.
   * If creating a new key, download the `.pem` or `.ppk` file instantly. *Note: You cannot download this file again later.*
2. **Network settings:**
   * Keep the default settings.
   * Ensure the checkbox for **Allow SSH traffic from (Anywhere / My IP)** is checked so you can access the command line later.

### Step 5: Final Review & Deployment
1. Verify your configuration details in the **Summary** sidebar on the right.
2. Click **Launch instance**.
3. Click **View all instances** at the bottom of the success page. 
4. Your server is live when the **Instance state** column switches from *Pending* to 🟢 **Running**.

---

## 📸 Phase 2: Creating a Custom AMI (Image) from an Instance

An AMI serves as a blueprint of your configured server, allowing you to clone it perfectly later.

> ℹ️ **NOTE ON REBOOTS:** By default, AWS will reboot the instance during this process to ensure data integrity on the disk image. Plan for a few minutes of temporary downtime.

### Step 1: Select the Source Instance
1. Go to your **EC2 Dashboard** and click on **Instances (running)**.
2. Check the **selection box** next to the instance you want to backup/clone.

### Step 2: Trigger Image Creation
1. Click the **Actions** dropdown menu button at the top of the table.
2. Hover over **Image and templates** and click 📷 **Create image**.

### Step 3: Configure Image Details
1. **Image name:** Provide a descriptive name (e.g., `web-server-golden-image-v1`).
2. **Image description:** (Optional) Detail what software or updates are pre-installed.
3. **No reboot:** Leave unchecked (recommended) so AWS can safely create a clean image snapshot.
4. Click the orange **Create image** button at the bottom right.

### Step 4: Verify Image Status
1. On the left sidebar menu, scroll down to the **Images** section and click **AMIs**.
2. Locate your new image. It is ready to use when the **Status** column switches from *Pending* to 🟢 **Available**.

---

## 🛠️ Phase 3: Creating an EC2 Launch Template

Launch Templates store your configuration details (AMI, instance type, security groups) so you don't have to re-enter them manually every time you launch a new server.

### Step 1: Navigate to Templates
1. On the left sidebar menu of the EC2 Dashboard, under **Instances**, click **Launch Templates**.
2. Click the orange **Create launch template** button.

### Step 2: Template Identity
1. **Launch template name:** Enter a unique identifier (e.g., `Web-Server-Standard-Template`).
2. **Template version description:** Add context (e.g., `Base template using our custom Ubuntu v1 AMI`).

### Step 3: Define Hardware & Software Blueprints
1. **Application and OS Images (AMI):** 
   * Click the **My AMIs** tab to select the custom image you created in Phase 2, or choose a standard Quick Start AWS AMI.
2. **Instance type:** Select your standard size (e.g., `t3.micro`).
3. **Key pair:** Select the standard login key your team uses.

### Step 4: Network & Security Group Assignments
1. **Network settings:** Set the graphics to *VPC*.
2. **Security groups:** Select the firewall rules you defined earlier to allow traffic (like SSH/HTTP).

### Step 5: Save Template
1. Review your configured settings in the right-hand panel.
2. Click **Create launch template**. You can now launch new identical servers with a single click using this template.

---

## 🛑 Phase 4: Terminating an EC2 Instance

Follow these steps to permanently delete your virtual machine and stop all ongoing compute charges.

> ⚠️ **CRITICAL WARNING:** 
> Terminating an instance is **permanent and completely irreversible**. 
> * All data stored on the instance's local root drive will be deleted forever.
> * If you want to temporarily save your work without paying for active CPU hours, select **Stop instance** instead.

### Step 1: Select the Target Instance
1. Go to your **EC2 Dashboard** and click on **Instances (running)**.
2. Find your instance in the list.
3. Check the **selection box** on the far-left side of that instance's row.

### Step 2: Trigger Termination Command
1. Locate the **Instance state** dropdown menu button at the top of the table view.
2. Select 🟥 **Terminate instance** from the options.

### Step 3: Confirm Deletion
1. Review the pop-up warning dialog detailing data loss.
2. Click the red **Terminate** button to confirm the action.

### Step 4: Verify Success
1. Watch the **Instance state** column change status to *Shutting down*.
2. The lifecycle is complete when it officially reads 🪦 **Terminated**. 
3. *Note: The entry will automatically disappear from your dashboard view within a few hours.*

---
