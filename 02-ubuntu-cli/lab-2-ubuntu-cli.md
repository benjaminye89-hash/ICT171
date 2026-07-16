# Lab 2: Ubuntu CLI Familiarisation

**Date:** 05/07/2026  
**Duration:** 1.5 hours  
**Status:** Complete  

---

##  Objective
To become familiar with the Ubuntu Linux command-line interface (CLI) and learn basic navigation, file management, and system exploration commands.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- Bash terminal

---

##  Step-by-Step Process

### 1. Opened the Terminal
I opened the terminal by clicking on the terminal icon in the Ubuntu sidebar or pressing `Ctrl + Alt + T`.

### 2. Explored the Filesystem
I practiced navigating the Linux filesystem using the following commands:

```bash
pwd                     # Print working directory – shows where I am
ls                      # List files and directories in current location
ls -la                  # List all files (including hidden) with details
cd /                    # Change directory to root (/)
cd /home                # Move to /home directory
cd ~                    # Move to my home directory (/home/benji)
cd ..                   # Move up one directory level
3. Created and Managed Files/Directories
I practiced creating and managing files and folders:

bash
mkdir test_folder       # Created a new directory called test_folder
touch myfile.txt        # Created an empty file called myfile.txt
cp myfile.txt myfile_copy.txt  # Copied the file
mv myfile.txt myfile_renamed.txt  # Renamed the file
rm myfile_renamed.txt   # Removed (deleted) the file
rmdir test_folder       # Removed the empty directory
4. Viewed File Contents
I viewed and edited files using different commands:

bash
cat /etc/hosts          # Displayed the contents of the hosts file
less /etc/services      # Viewed the services file page by page
head /etc/passwd        # Showed the first 10 lines of passwd file
tail /var/log/syslog    # Showed the last 10 lines of the system log
nano myfile.txt         # Opened a file in the nano text editor
5. Explored the Linux Directory Structure
I explored key Linux directories:

Directory	Purpose
/	Root directory – the top of the filesystem
/home/	User home directories
/etc/	System configuration files
/var/	Variable data (logs, mail, etc.)
/bin/	Essential system binaries/commands
/tmp/	Temporary files
6. Used the man Command
I used the man (manual) command to learn about other commands:

bash
man ls                  # Showed the manual page for 'ls'
man cp                  # Showed the manual page for 'cp'
man mkdir               # Showed the manual page for 'mkdir'
7. Checked System Information
I used commands to view system information:

bash
whoami                  # Showed my current username
id                      # Showed my user ID and group information
uname -a                # Showed system kernel information
hostname                # Showed the computer's hostname


⚠️ Errors Faced
Error 1: Permission denied when viewing some system files
Error Message: cat: /etc/shadow: Permission denied

Why it happened: Some system files are protected and require root privileges to view.

How I fixed it: Used sudo to view protected files:

bash
sudo cat /etc/shadow
What I learned: The sudo command grants temporary administrative privileges.

🧠 Reflection
What I Learned
The Linux filesystem is hierarchical, starting from the root / directory.

pwd, ls, and cd are essential for navigating the filesystem.

Hidden files (starting with .) can be viewed with ls -la.

man pages are incredibly useful for learning new commands.

sudo is required for system-level operations.

Real-World Application
System administrators use these basic CLI commands daily to manage servers.

Understanding the filesystem structure is essential for configuring services.

Remote server management (SSH) relies on these same CLI skills.
