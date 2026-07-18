# Lab 7: Cloud Computing (AWS EC2)

**Date:** 15/07/2026  
**Duration:** 2 hours  
**Status:** Complete  

---

## 🎯 Objective
To provision, configure, and secure a cloud-based virtual machine using Amazon Web Services (AWS) EC2, and connect to it via SSH.

---

## 🛠️ Tools Used
- AWS Free Tier Account
- AWS EC2 (t2.micro instance)
- Ubuntu 24.04 LTS AMI
- SSH keypair
- Security Groups (Firewall)
- PuTTY / OpenSSH client

---

## 📋 Step-by-Step Process

### 1. Created AWS Free Tier Account
I went to https://aws.amazon.com/free and signed up for a free tier account. I provided my email, payment details (for verification), and completed the registration process.

### 2. Launched an EC2 Instance
I navigated to the EC2 dashboard and launched a new instance:

| Setting | Value |
|---------|-------|
| **AMI** | Ubuntu 24.04 LTS (HVM) |
| **Instance Type** | t2.micro (Free Tier eligible) |
| **VPC** | Default VPC |
| **Subnet** | Default subnet (ap-southeast-1a) |
| **Storage** | 20GB gp2 (General Purpose SSD) |
| **Key Pair** | Created new key pair: `ict171-key` |

### 3. Created and Downloaded a Key Pair
- Clicked **"Create new key pair"**
- Named it: `ict171-key`
- Type: RSA (.pem format)
- Downloaded the `.pem` file to my local machine
- Moved it to `~/.ssh/` and set correct permissions:
  ```bash
  mv ~/Downloads/ict171-key.pem ~/.ssh/
  chmod 400 ~/.ssh/ict171-key.pem
4. Configured Security Groups (Firewall)
I created a security group to control inbound traffic:

Rule	Port	Protocol	Source	Purpose
SSH	22	TCP	0.0.0.0/0	Remote access (later restricted)
HTTP	80	TCP	0.0.0.0/0	Web traffic
HTTPS	443	TCP	0.0.0.0/0	Secure web traffic
5. Launched the Instance
I clicked "Launch Instance" and waited for the instance state to change from "Pending" to "Running".

6. Connected via SSH
Once running, I connected to the instance using SSH:

bash
# From Linux/Mac terminal or Windows (with OpenSSH)
ssh -i ~/.ssh/ict171-key.pem ubuntu@<PUBLIC_IP>

# Example:
ssh -i ~/.ssh/ict171-key.pem ubuntu@54.169.123.45
7. Updated the System
After successful connection, I updated the package list and upgraded packages:

bash
sudo apt update && sudo apt upgrade -y
8. Installed Apache Web Server
I installed Apache to test the server was working:

bash
sudo apt install apache2 -y
sudo systemctl start apache2
sudo systemctl enable apache2
9. Verified Web Server
I opened a browser and visited http://<PUBLIC_IP> to confirm Apache was running. The default Ubuntu Apache welcome page appeared.

10. Created a Custom Webpage
I created a simple custom webpage:

bash
echo "<h1>Hello from AWS EC2!</h1>" | sudo tee /var/www/html/index.html
11. Monitored Instance Costs
I checked the AWS Cost Explorer to monitor usage:

Resource	Monthly Cost (Est.)
t2.micro (750 hours free)	$0.00
20GB Storage (gp2)	~$2.00
Total	~$2.00/month
12. Stopped the Instance
To avoid incurring charges, I stopped the instance when not in use:

bash
# From AWS Console: Actions → Instance State → Stop

⚠️ Errors Faced
Error 1: SSH Connection Refused
Error Message: ssh: connect to host 54.169.123.45 port 22: Connection refused

Why it happened: The security group did not allow inbound SSH traffic (port 22).

How I fixed it: Added a rule to the security group:

Type: SSH, Port: 22, Source: 0.0.0.0/0

What I learned: Security groups are stateful firewalls; you must explicitly allow inbound traffic.

Error 2: Permission Denied (Public Key)
Error Message: Permission denied (publickey)

Why it happened: The .pem file had incorrect permissions.

How I fixed it: Set the correct permissions:

bash
chmod 400 ~/.ssh/ict171-key.pem
What I learned: SSH requires strict key permissions (400 or 600).

Error 3: Instance IP Kept Changing
Problem: The public IP changed every time I stopped/started the instance.

Why it happened: By default, EC2 uses dynamic public IPs (unless you allocate an Elastic IP).

How I fixed it: Noted the new IP each time or allocated an Elastic IP (limited free tier).

What I learned: Dynamic IPs are normal; for production, use Elastic IPs or DNS.

Error 4: Instance Not Responding to HTTP
Error Message: Unable to connect (browser)

Why it happened: The security group did not allow HTTP traffic (port 80).

How I fixed it: Added HTTP rule to the security group.

What I learned: Web servers need port 80 (HTTP) and 443 (HTTPS) open.

🧠 Reflection
What I Learned
Cloud VMs are provisioned in minutes, compared to physical servers which take weeks.

AWS EC2 offers free tier instances for learning and testing.

Security groups are AWS's firewall solution – they control inbound/outbound traffic.

SSH key pairs are the preferred method for secure authentication in the cloud.

Infrastructure as Code (IaC) concepts: everything is defined through the console (or APIs).

Elastic IPs provide static public IP addresses for persistent hosting.

Key Commands Summary
Command	Purpose
ssh -i ~/.ssh/key.pem ubuntu@<IP>	Connect to EC2 instance
sudo apt update && sudo apt upgrade -y	Update system packages
sudo apt install apache2 -y	Install Apache web server
sudo systemctl start apache2	Start Apache service
sudo systemctl enable apache2	Enable Apache at boot
chmod 400 ~/.ssh/key.pem	Set correct permissions for key file
Real-World Application
Cloud computing is the standard for modern IT infrastructure.

AWS is the most popular cloud provider – understanding EC2 is a valuable skill.

Cost management is critical – always stop instances when not in use.

Security is fundamental – limit SSH access to your IP and use strong key pairs.

Elastic IPs are used for production servers that need a consistent IP address.
