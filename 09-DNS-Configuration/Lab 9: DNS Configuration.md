# Lab 9: DNS Configuration

**Date:** 15/07/2026  
**Duration:** 2 hours  
**Status:** Complete  

---

## 🎯 Objective
To understand and configure Domain Name System (DNS) services, including setting up A records, testing resolution using `dig` and `nslookup`, and exploring DNS propagation.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- AWS Route 53 / Cloud DNS
- `dig`, `nslookup`, `host` commands
- Domain registration service (Freenom)

---

## 📋 Step-by-Step Process

### 1. Understood DNS Concepts
I reviewed the basics of DNS:

| Term | Definition |
|------|------------|
| **DNS** | Domain Name System – translates human-readable domain names to IP addresses |
| **Domain** | e.g., `example.com` |
| **A Record** | Maps a domain to an IPv4 address |
| **AAAA Record** | Maps a domain to an IPv6 address |
| **CNAME Record** | Maps a domain to another domain name |
| **MX Record** | Mail exchange – directs email to a mail server |
| **NS Record** | Name server – identifies authoritative DNS servers |
| **TTL** | Time to Live – how long a record is cached |

### 2. Registered a Domain (Free Domain)
I registered a free domain using Freenom:

| Step | Action |
|------|--------|
| 1 | Went to https://www.freenom.com |
| 2 | Searched for available free domains (e.g., `.tk`, `.ml`, `.ga`, `.cf`, `.gq`) |
| 3 | Selected a domain: `mycloudserver.tk` |
| 4 | Completed registration (free for 12 months) |

### 3. Created an A Record
I created an A record pointing to my AWS EC2 instance public IP:

| Record Type | Name | Value (IP) | TTL |
|-------------|------|------------|-----|
| A | `mycloudserver.tk` | `54.169.123.45` | 300 |

### 4. Verified DNS Propagation with `dig`
I used `dig` to check DNS resolution:

```bash
# Query the A record
dig mycloudserver.tk

# Query specific DNS server (Google's public DNS)
dig @8.8.8.8 mycloudserver.tk

# Query with short output
dig mycloudserver.tk +short
Output Example:

text
; <<>> DiG 9.18.28 <<>> mycloudserver.tk
;; global options: +cmd
;; Got answer:
;; ->>HEADER<<- opcode: QUERY, status: NOERROR, id: 12345
;; flags: qr rd ra; QUERY: 1, ANSWER: 1, AUTHORITY: 0, ADDITIONAL: 1

;; ANSWER SECTION:
mycloudserver.tk.  300  IN  A  54.169.123.45
5. Verified DNS with nslookup
I used nslookup to resolve the domain:

bash
nslookup mycloudserver.tk
Output Example:

text
Server:         127.0.0.53
Address:        127.0.0.53#53

Non-authoritative answer:
Name:   mycloudserver.tk
Address: 54.169.123.45
6. Used host Command
I used the host command for quick lookups:

bash
host mycloudserver.tk
Output Example:

text
mycloudserver.tk has address 54.169.123.45
7. Checked Propagation Using Public DNS Servers
I checked if DNS propagated to different public DNS servers:

bash
# Check using Google DNS
dig @8.8.8.8 mycloudserver.tk +short

# Check using Cloudflare DNS
dig @1.1.1.1 mycloudserver.tk +short

# Check using OpenDNS
dig @208.67.222.222 mycloudserver.tk +short
8. Performed a Reverse DNS Lookup
I performed a reverse DNS lookup (PTR record):

bash
dig -x 54.169.123.45
9. Traced DNS Resolution Path
I traced the DNS resolution path using dig +trace:

bash
dig +trace mycloudserver.tk
10. Modified Local DNS (/etc/hosts)
I edited the local hosts file to test DNS locally:

bash
sudo nano /etc/hosts
Added this line:

text
54.169.123.45  mycloudserver.tk
Verified with ping:

bash
ping mycloudserver.tk -c 4
11. DNS Propagation Check
I used online tools to check propagation from multiple locations:

Tool	URL
DNS Checker	https://dnschecker.org
What's My DNS	https://whatsmydns.net
MX Toolbox	https://mxtoolbox.com/DNSLookup.aspx

⚠️ Errors Faced
Error 1: NXDOMAIN - Domain Not Found
Error Message: ;; ->>HEADER<<- opcode: QUERY, status: NXDOMAIN

Why it happened: The A record was not created correctly or TTL was not propagated yet.

How I fixed it: Verified the record was created, waited for TTL (5 minutes), and rechecked. If using Freenom, ensured the nameservers were set correctly.

What I learned: DNS propagation can take up to 24-48 hours, though usually 5-30 minutes.

Error 2: Wrong Nameserver Settings
Error Message: (Domain not resolving)

Why it happened: Freenom default nameservers were not pointing to AWS Route 53.

How I fixed it: Changed nameservers in Freenom dashboard:

ns-123.awsdns-45.com

ns-456.awsdns-78.net

What I learned: You must set the correct nameservers for your domain provider.

Error 3: Permission Denied Editing /etc/hosts
Error Message: sudo: /etc/hosts: Permission denied

Why it happened: I forgot sudo when editing the protected file.

How I fixed it: Used sudo nano /etc/hosts to edit with proper permissions.

What I learned: System files require sudo for editing.

🧠 Reflection
What I Learned
DNS translates human-readable domain names to machine-readable IP addresses.

A Records map domain names to IPv4 addresses.

TTL (Time to Live) controls how long a record is cached – shorter TTL = faster changes.

DNS propagation can take up to 48 hours; use TTL=300 (5 mins) for testing.

dig and nslookup are essential tools for DNS troubleshooting.

Reverse DNS resolves IP addresses to hostnames.

Local DNS can be overridden using /etc/hosts for testing.

Key Commands Summary
Command	Purpose
dig domain.com	Resolve domain to IP address
dig @8.8.8.8 domain.com	Resolve using a specific DNS server
dig domain.com +short	Short output (just the IP)
dig -x IP_ADDRESS	Reverse DNS lookup
dig +trace domain.com	Trace the DNS resolution path
nslookup domain.com	Resolve domain (interactive/command-line)
host domain.com	Quick domain lookup
ping domain.com	Test DNS resolution and network connectivity
sudo nano /etc/hosts	Edit local DNS mapping
Real-World Application
DNS is the backbone of the internet – without it, we'd need to remember IP addresses.

System administrators use DNS to route traffic to the correct servers.

Cloud engineers configure DNS to point domains to load balancers and CDNs.

Low TTL is used for critical changes (e.g., failover) to ensure fast propagation.
