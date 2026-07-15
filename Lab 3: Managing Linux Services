# Lab 3: Managing Linux Services

**Date:** 15/07/2026  
**Duration:** 1.5 hours  
**Status:** Complete  

---

##  Objective
To understand and manage background services (daemons) in Linux using `systemctl`, the systemd service manager.

---

##  Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- `systemctl` command
- Apache2 web server

---

##  Step-by-Step Process

### 1. Checked All Services on the System
I listed all active and inactive services running on my system:

```bash
systemctl list-units --type=service
This showed me all services, their status (active/inactive), and whether they were enabled to start at boot.

2. Checked Status of a Specific Service
I checked the status of the SSH service to see if it was running:

bash
systemctl status ssh
The output showed:

Active state: active (running) or inactive

Process ID (PID): The running process number

Memory usage: How much memory the service was using

Recent logs: The last few log entries for that service

3. Installed Apache Web Server
I installed Apache (a common web server) to practice managing services:

bash
sudo apt update
sudo apt install apache2 -y
4. Started the Apache Service
After installation, I started the Apache service:

bash
sudo systemctl start apache2
5. Checked Apache Status
I verified Apache was running:

bash
sudo systemctl status apache2
6. Tested Apache in Browser
I opened a browser and visited http://127.0.0.1 to confirm Apache was working. The default Ubuntu Apache welcome page appeared.

7. Enabled Apache to Auto-Start at Boot
I configured Apache to start automatically every time my VM boots:

bash
sudo systemctl enable apache2
8. Stopped and Restarted Apache
I practiced stopping and restarting the service:

bash
sudo systemctl stop apache2    # Stopped Apache
sudo systemctl status apache2  # Confirmed it was stopped
sudo systemctl start apache2   # Started it again
sudo systemctl restart apache2 # Restarted it (for configuration changes)
9. Checked Listening Ports
I checked that Apache was listening on port 80 (HTTP):

bash
sudo netstat -tulpn | grep :80
