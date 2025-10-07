Виконала Самостійно Слобожан Софія РПЗ 24-А

## Overview
This lab focuses on exploring Amazon Web Services (AWS), specifically the process of launching and setting up virtual machines known as EC2 instances. The goal is to gain hands-on experience with AWS cloud infrastructure, including instance creation, configuration, and basic management.

## Pre-Lab Knowledge Check

### Role of AMIs and Their Accessibility
Amazon Machine Images (AMIs) serve as pre-configured templates for launching EC2 instances, encapsulating the operating system, software, and settings needed to boot a virtual server.  
Public AMIs are openly accessible to any AWS account holder, promoting reusability across the community. In contrast, private AMIs are restricted to the creating account or shared within an organization, ideal for custom or sensitive setups.

### Functionality of EBS and Available Volume Types
Elastic Block Store (EBS) offers durable, block-level storage volumes that attach to EC2 instances, ensuring data persistence even after instance shutdowns.  
Supported EBS volume types include:  
- General Purpose SSD (gp2 or gp3): Versatile for everyday workloads with balanced performance.  
- Provisioned IOPS SSD (io1 or io2): High-performance for I/O-intensive applications like databases.  
- Throughput Optimized HDD (st1): Cost-effective for big data streaming and logs.  
- Cold HDD (sc1): Economical for infrequently accessed data backups.

### Purpose of Snapshots
EBS snapshots capture the exact state of a volume at a specific moment, enabling point-in-time backups. They facilitate data recovery, volume cloning, or migration to new instances.

### Overview of EC2 Instance Families and Use Cases
AWS categorizes EC2 instances by workload needs:  
- **General Purpose** (e.g., t3 or m5 series): Suited for standard applications requiring equilibrium across CPU, memory, and networking, like web hosting or development environments.  
- **Compute Optimized** (e.g., c5 series): Ideal for CPU-bound tasks such as batch processing, gaming servers, or scientific simulations.  
- **Memory Optimized** (e.g., r5 or x1 series): Best for memory-heavy operations like large-scale databases, caching, or real-time analytics.  
- **Storage Optimized** (e.g., i3 or d2 series): Designed for high-throughput storage tasks, including NoSQL databases or data warehousing.  
- **Accelerated Computing** (e.g., g4 or p3 series): Optimized for graphics rendering, machine learning, or high-performance computing with GPU/FPGA support.

### Free Tier OS Options
AWS Free Tier AMIs support various operating systems with eligible configurations (e.g., 750 hours/month on t2.micro/t3.micro).  
- **Linux Family**: Amazon Linux 2 – A secure, AWS-optimized distro with kernel tweaks for cloud efficiency; typically 1 vCPU and 1 GiB RAM, great for scripting and automation.  
- **Windows Family**: Windows Server 2019 – Features a full desktop experience with Active Directory support; 1 vCPU and 1 GiB RAM, suitable for .NET apps or testing.  
- **macOS Family**: macOS Big Sur – Tailored for Xcode and iOS development; requires dedicated hardware (mac1.metal), with higher baseline resources and costs outside Free Tier.

## Hands-On Exercise 1: Setting Up a Linux Instance

1. Accessed the AWS Management Console and navigated to the EC2 Dashboard.  
2. Selected "Launch Instance" to begin the wizard.  
3. Named the instance "Ubuntu-EC2-Experiment" and chose the Amazon Linux 2 AMI (Free Tier eligible).  
4. Selected t3.micro as the instance type to stay within Free Tier limits.  
5. Generated a new key pair named "lab-key-pair" for secure SSH access and downloaded the .pem file.  
6. Configured the security group to permit inbound traffic for SSH (port 22), HTTP (port 80), and HTTPS (port 443) from anywhere (0.0.0.0/0).  
7. Set the root volume to 8 GiB using gp3 for general-purpose storage.  
8. In the Advanced Details section, added a user data script to automate Apache installation:  
   ```
   #!/bin/bash
   sudo yum update -y
   sudo yum install httpd -y
   sudo systemctl start httpd
   sudo systemctl enable httpd
   ```  
9. Reviewed and launched the instance.  
10. Once running, retrieved the public IP and verified the web server by accessing `http://<public-ip>` in a browser, confirming the default Apache page.  

**Screenshots** (attached in repo):  
- Instance launch configuration: `images/linux-launch.png`  
- Successful Apache page: `images/apache-test.png`

## Hands-On Exercise 2: Configuring a Windows Instance

1. From the EC2 Dashboard, initiated a new instance launch.  
2. Named it "WinServer-Test" and selected the Windows Server 2019 Base AMI.  
3. Chose t3.micro for Free Tier compatibility.  
4. Created a new key pair ("win-lab-key") and applied default VPC/subnet settings.  
5. Allowed RDP (port 3389) in the security group from my IP only for security.  
6. Launched with the default 30 GiB gp3 root volume.  
7. After launch, decrypted the administrator password using the private key via the EC2 console.  
8. Connected using Microsoft Remote Desktop with the public IP, username "Administrator", and decrypted password.  
9. In the RDP session, accessed System Properties (via `sysdm.cpl`), changed the computer name to "Cloud-Win-Lab", and applied the changes.  
10. Restarted the instance remotely and reconnected to verify the new hostname via `hostname` command in PowerShell.  

**Screenshots** (attached in repo):  
- Instance details page: `images/windows-launch.png`  
- RDP connection screen: `images/rdp-connect.png`  
- Updated system name: `images/hostname-change.png`

## Post-Lab Review Questions

### Importance of Key Pairs in EC2 Access
Key pairs provide asymmetric encryption for secure logins: the public key is stored on the instance, while the private key (kept by the user) authenticates SSH or decrypts RDP passwords, preventing unauthorized access.

### SSH Connection Command for Linux Instances
To connect:  
`ssh -i /path/to/lab-key.pem ec2-user@<public-dns-or-ip>`

### Methods for Linux EC2 Connectivity
Linux instances are accessed via SSH clients like OpenSSH (terminal), PuTTY, or Git Bash, requiring the private key file and the instance's public DNS/IP.

### Methods for Windows EC2 Connectivity
Windows instances use RDP clients (e.g., Windows Remote Desktop or macOS Screen Sharing). Obtain the password by decrypting it in the AWS console with your private key, then connect to the public IP on port 3389.

### Distinction Between Stop and Terminate Actions
- **Stop**: Shuts down the instance temporarily, preserving EBS data and allowing restarts (billed only for storage).  
- **Terminate**: Deletes the instance entirely, including attached EBS volumes (unless configured otherwise), making recovery impossible.

## Summary
This exercise introduced me to AWS EC2 fundamentals, from selecting AMIs and instance types to secure connections via SSH/RDP and basic automation with user data scripts. I successfully deployed a Linux server with a web component and customized a Windows environment, reinforcing concepts in cloud provisioning, security, and lifecycle management. These skills are foundational for scalable infrastructure in AWS.

---

