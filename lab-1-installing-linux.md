# Lab 1: Installing Linux on My PC

**Date:** 04/07/2026  
**Duration:** 2 hours  
**Status:** Complete  

---

##  Objective
To install Ubuntu 24.04 LTS on my PC using VirtualBox, creating a virtualised Linux environment for the unit's labs.

---

##  Tools Used
- VirtualBox 7.0
- Ubuntu 24.04 LTS ISO

---

##  Step-by-Step Process

### 1. Downloaded VirtualBox
I went to https://www.virtualbox.org/wiki/Downloads and downloaded the Windows version. Installed it by following the default setup.

### 2. Downloaded Ubuntu ISO
I downloaded Ubuntu 24.04 LTS from the official Ubuntu website.

### 3. Created the VM in VirtualBox
- Clicked "New" in VirtualBox
- Named it "Ubuntu 24.04"
- Allocated **4GB RAM**
- Allocated **20GB disk space**
- Mounted the Ubuntu ISO as a virtual CD/DVD

### 4. Installed Ubuntu
I started the VM and followed the guided installer:
- Selected language: English
- Chose "Normal installation"
- Created username/password
- Let it install (took about 10-15 minutes)

### 5. Updated the System
After installation, I opened the terminal and ran:
```bash
sudo apt update && sudo apt upgrade -y<img width="598" height="427" alt="Screenshot 2026-07-15 212536" src="https://github.com/user-attachments/assets/f7b19bb9-5b23-4599-b9ac-8eaec48bf7fb" />


<img width="598" height="427" alt="Screenshot 2026-07-15 212536" src="https://github.com/user-attachments/assets/25122986-8e85-4d18-958c-ec5e50cd4521" />
