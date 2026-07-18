# Lab 10: Digital Certificates with Let's Encrypt

**Date:** 15/07/2026  
**Duration:** 2 hours  
**Status:** Complete  

---

## 🎯 Objective
To obtain and manage digital certificates using Let's Encrypt (Certbot) to secure a web server with HTTPS, enabling encrypted communication between clients and the server.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- Apache2 / Nginx web server
- Certbot (Let's Encrypt client)
- Domain name (from Lab 9)
- AWS EC2 instance (or local VM)

---

## 📋 Step-by-Step Process

### 1. Understood Digital Certificates
I reviewed the basics of SSL/TLS and digital certificates:

| Term | Definition |
|------|------------|
| **SSL/TLS** | Secure Sockets Layer / Transport Layer Security – protocols for encrypted communication |
| **Certificate** | A digital file that binds a public key to a domain name |
| **CA** | Certificate Authority – trusted entity that issues certificates |
| **Let's Encrypt** | Free, automated, open-source CA |
| **Certbot** | Automated client for Let's Encrypt |
| **HTTPS** | HTTP over SSL/TLS (port 443) |
| **HTTP** | Unencrypted protocol (port 80) |

### 2. Ensured Apache/Nginx was Installed
I verified my web server was running:

```bash
# Check Apache
sudo systemctl status apache2

# Or if using Nginx
sudo systemctl status nginx
3. Installed Certbot
I installed the Certbot package for my web server:

For Apache:

bash
sudo apt update
sudo apt install certbot python3-certbot-apache -y
For Nginx:

bash
sudo apt update
sudo apt install certbot python3-certbot-nginx -y
4. Obtained a Certificate
I ran Certbot to obtain and install the certificate:

For Apache:

bash
sudo certbot --apache
For Nginx:

bash
sudo certbot --nginx
Prompts and my responses:

Prompt	My Response
Email address	benjamin.ye@murdoch.edu.au
Terms of Service	A (Agree)
Share email with EFF	Y or N
Domain name(s)	mycloudserver.tk
5. Verified the Certificate
I verified the certificate was installed and working:

bash
sudo certbot certificates
Output Example:

text
Found the following certs:
  Certificate Name: mycloudserver.tk
    Domains: mycloudserver.tk
    Expiry Date: 2026-10-13 14:30:00+00:00 (VALID: 90 days)
    Certificate Path: /etc/letsencrypt/live/mycloudserver.tk/fullchain.pem
    Private Key Path: /etc/letsencrypt/live/mycloudserver.tk/privkey.pem
6. Tested HTTPS in Browser
I opened a browser and visited:

https://mycloudserver.tk

Result: The padlock icon appeared in the address bar, indicating a secure connection.

7. Explored the Certificate Details
I viewed the certificate details in the browser:

Field	Value
Issuer	Let's Encrypt
Validity	90 days
Subject	CN = mycloudserver.tk
Signature Algorithm	SHA-256 with RSA
Serial Number	(Unique serial number)
8. Configured Auto-Renewal
I verified that auto-renewal was configured:

bash
sudo systemctl status certbot.timer
Output Example:

text
● certbot.timer - Run certbot twice daily
   Loaded: loaded (/lib/systemd/system/certbot.timer; enabled)
   Active: active (waiting) since Wed 2026-07-15 14:30:00 +08; 1h ago
9. Tested Renewal Process
I performed a dry-run of the renewal process:

bash
sudo certbot renew --dry-run
Output:

text
** DRY RUN: simulating 'certbot renew' close to cert expiry
** (The test certificates below have not been saved.)

Congratulations, all simulated renewals succeeded:
  /etc/letsencrypt/live/mycloudserver.tk/fullchain.pem (success)
10. Redirected HTTP to HTTPS
I configured the web server to automatically redirect HTTP to HTTPS:

Apache: It's automatically configured by Certbot for --apache.

Nginx: Certbot adds redirect rules automatically. To verify:

bash
# Check Apache config
sudo cat /etc/apache2/sites-available/000-default-le-ssl.conf | grep "ServerName"
11. Checked Security Headers
I checked the security headers using online tools:

Tool	URL
SSL Labs	https://www.ssllabs.com/ssltest/
Security Headers	https://securityheaders.com

⚠️ Errors Faced
Error 1: Domain Not Resolving
Error Message: No valid domain names found. Check that the domain exists and is correctly configured.

Why it happened: The domain name did not resolve to the server's public IP.

How I fixed it: Created an A record in my DNS provider pointing to the server IP and waited for propagation (TTL=300).

What I learned: Certbot requires the domain to resolve to the server IP to issue a certificate.

Error 2: Port 80 Not Open
Error Message: Connection refused: port 80 is not open.

Why it happened: The firewall (UFW) blocked port 80, or the web server wasn't running.

How I fixed it: Opened the port and started the web server:

bash
sudo ufw allow 80
sudo ufw allow 443
sudo systemctl start apache2
What I learned: Let's Encrypt uses port 80 for HTTP-01 challenge validation.

Error 3: Certificate Already Exists
Error Message: The certificate for mydomain.com already exists.

Why it happened: I had already issued a certificate for that domain.

How I fixed it: Used --force-renewal flag:

bash
sudo certbot renew --force-renewal
What I learned: Certbot prevents duplicate certificates by default.

Error 4: SELinux Blocking Apache
Error Message: (Apache not serving HTTPS despite certificate being installed)

Why it happened: SELinux was blocking Apache from reading the certificate files.

How I fixed it: Restored SELinux context or used sudo setenforce 0 (temporarily).

What I learned: SELinux can interfere with web server file access.

🧠 Reflection
What I Learned
SSL/TLS certificates encrypt data between clients and servers, preventing eavesdropping.

Let's Encrypt provides free, automated certificates valid for 90 days.

Certbot automates certificate issuance, installation, and renewal.

Auto-renewal is critical to avoid expired certificates (which cause security warnings).

Port 80 must be open for certificate issuance (HTTP-01 challenge).

HTTPS is now the standard for all web traffic; HTTP is deprecated.

Key Commands Summary
Command	Purpose
sudo apt install certbot python3-certbot-apache	Install Certbot for Apache
sudo apt install certbot python3-certbot-nginx	Install Certbot for Nginx
sudo certbot --apache	Issue and install certificate for Apache
sudo certbot --nginx	Issue and install certificate for Nginx
sudo certbot certificates	List all certificates
sudo certbot renew --dry-run	Test renewal process
sudo certbot renew --force-renewal	Force immediate renewal
sudo systemctl status certbot.timer	Check auto-renewal status
sudo ufw allow 443	Open HTTPS port in firewall
Real-World Application
HTTPS is mandatory for e-commerce, banking, and any site handling sensitive data.

Let's Encrypt is widely used in DevOps and cloud environments.

Auto-renewal ensures uninterrupted service and reduces manual admin effort.

Security headers (HSTS, etc.) improve site security beyond just installing a certificate.
