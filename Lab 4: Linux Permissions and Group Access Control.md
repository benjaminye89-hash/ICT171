# Lab 4: Linux Permissions and Group Access Control

**Date:** 12/07/2026  
**Duration:** 1.5 hours  
**Status:** Complete  

---

## 🎯 Objective
To understand and apply Linux file and directory permissions, manage users and groups, and control access using `chmod`, `chown`, and `chgrp`.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- `ls -l`, `chmod`, `chown`, `chgrp`
- `adduser`, `groupadd`, `usermod`

---

## 📋 Step-by-Step Process

### 1. Created Users
I created three test users to practice permissions:

```bash
sudo adduser alice
sudo adduser bob
sudo adduser mallory
I set passwords for each user when prompted.

2. Created a Group
I created a group called sharedgroup:

bash
sudo groupadd sharedgroup
3. Added Users to the Group
I added Alice and Bob to the group:

bash
sudo usermod -aG sharedgroup alice
sudo usermod -aG sharedgroup bob
Verified the group membership:

bash
groups alice
groups bob
4. Created a Shared Directory
I created a directory for shared files:

bash
sudo mkdir /home/shared
5. Created Test Files
I created 10 test files inside the shared directory:

bash
sudo touch /home/shared/file{1..10}
6. Changed Group Ownership
I changed the group ownership of the shared directory and all its files:

bash
sudo chgrp -R sharedgroup /home/shared
7. Set Directory Permissions
I set permissions so that:

Owner (root) has read, write, execute (7)

Group members (Alice and Bob) have read, write, execute (7)

Others (Mallory) have no access (0)

bash
sudo chmod -R 770 /home/shared
8. Verified Permissions
I checked the permissions:

bash
ls -la /home/shared
Output showed:

text
drwxrwx--- 2 root sharedgroup 4096 Jul 15 12:00 .
-rw-rw---- 1 root sharedgroup    0 Jul 15 12:00 file1
-rw-rw---- 1 root sharedgroup    0 Jul 15 12:00 file2
...
9. Tested Access as Each User
I switched to each user to test access:

bash
# As Alice (should have access)
su - alice
cd /home/shared
touch alice_file.txt    # Works! ✅
ls -la                  # Can see all files
exit

# As Bob (should have access)
su - bob
cd /home/shared
touch bob_file.txt      # Works! ✅
exit

# As Mallory (should be denied)
su - mallory
cd /home/shared         # Permission denied! ❌
ls /home/shared         # Permission denied! ❌
exit
10. Challenged: Override Bob's Rights
I removed write access for Bob (keeping read and execute only):

bash
sudo chmod 750 /home/shared
Verified Bob's new permissions:

bash
su - bob
cd /home/shared         # Works (execute)
ls -la                  # Works (read)
touch bob_new.txt       # Permission denied! ❌
exit
11. Challenged: Granted Mallory Sudo Access
I added Mallory to the sudo group:

bash
sudo usermod -aG sudo mallory
Tested Mallory's new access:

bash
su - mallory
sudo ls /root           # Works! Has admin access now
exit
12. Cleanup
I removed the shared directory and all test files:

bash
sudo rm -r /home/shared


⚠️ Errors Faced
Error 1: Permission Denied When Creating Users
Error Message: adduser: Permission denied

Why it happened: Creating users requires administrative privileges.

How I fixed it: Used sudo before each user creation command.

What I learned: Only root and sudo users can create/modify users and groups.

Error 2: Mallory Could Still See Files
Error Message: (No error, but unexpected access)

Why it happened: The shared directory was in /home/ which has default world-readable permissions.

How I fixed it: Changed the group ownership and set chmod 770 to restrict access.

What I learned: Default permissions can be insecure; always explicitly set permissions.

🧠 Reflection
What I Learned
Linux permissions are based on three categories: Owner, Group, and Others.

Permissions are: r (read), w (write), x (execute).

Numeric permissions (e.g., chmod 770) are faster than symbolic (chmod u+rwx,g+rwx,o-rwx).

chown changes file/directory owner; chgrp changes group ownership.

Groups are a scalable way to manage multiple users with similar access needs.

sudo grants administrative privileges and should be assigned carefully.

Real-World Application
System administrators use permissions to secure sensitive files and directories.

Multi-user environments (like university servers) rely on groups to manage access.

Granting sudo access should be limited to trusted users to prevent security breaches.

Understanding the numeric permission system is essential for scripting and automation.
