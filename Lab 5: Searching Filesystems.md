# Lab 5: Searching Filesystems

**Date:** 15/07/2026  
**Duration:** 1.5 hours  
**Status:** Complete  

---

## 🎯 Objective
To learn how to search for files and content within files using powerful Linux command-line tools: `find`, `grep`, `locate`, and `which`.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- `find`, `grep`, `locate`, `which`, `whereis`

---

## 📋 Step-by-Step Process

### 1. Located the `find` Command
I checked where the `find` command is located on the system:

```bash
which find
# Output: /usr/bin/find

whereis find
# Output: find: /usr/bin/find /usr/share/man/man1/find.1.gz
2. Searched for Files by Name
I searched for files with specific names and extensions:

bash
# Search for all .txt files in the current directory and subdirectories
find . -name "*.txt"

# Search for files named "hosts" in the /etc directory
find /etc -name "hosts"

# Case-insensitive search for "readme" files
find . -iname "*readme*"
3. Searched by Type
I searched for specific types of files:

bash
# Search for directories named "logs"
find / -type d -name "logs" 2>/dev/null

# Search for regular files (not directories) with .conf extension
find /etc -type f -name "*.conf"

# Search for symbolic links
find / -type l -name "*.so" 2>/dev/null
4. Searched by Size
I found files based on their size:

bash
# Find files larger than 100MB
find / -type f -size +100M 2>/dev/null

# Find files smaller than 1KB
find / -type f -size -1k

# Find files exactly 255258 bytes (for the Gutenberg lab)
find . -type f -size 255258c
5. Searched by Modification Time
I found files based on when they were modified:

bash
# Find files modified in the last 7 days
find /home -type f -mtime -7

# Find files modified more than 30 days ago
find /var/log -type f -mtime +30

# Find files accessed in the last 24 hours
find / -type f -atime -1 2>/dev/null
6. Used locate for Fast Searching
I used the locate command (which uses a pre-built database) for faster searches:

bash
# Install locate (if not already installed)
sudo apt install plocate -y

# Update the locate database
sudo updatedb

# Search for files containing "apache2" in the name
locate apache2

# Search for all .conf files
locate "*.conf"
7. Searched Inside Files with grep
I used grep to search for text patterns inside files:

bash
# Search for the word "error" in all files in /var/log/
grep "error" /var/log/*

# Recursively search for "192.168.1" in /etc/
grep -r "192.168.1" /etc/

# Show line numbers and ignore case
grep -ni "warning" /var/log/syslog

# Search for lines NOT containing "success"
grep -v "success" /var/log/apache2/access.log
8. Used grep with Options
I practiced different grep options:

bash
# Search with context (3 lines before and after)
grep -C 3 "ERROR" /var/log/syslog

# Search with just the filename (no line content)
grep -l "root" /etc/*

# Count occurrences in a file
grep -c "error" /var/log/syslog
9. Combined find and grep
I searched for files by name and then searched inside them:

bash
# Find all .log files and search for "ERROR" inside them
find /var/log -name "*.log" -exec grep -H "ERROR" {} \;

# Find all .conf files and search for "port" inside them
find /etc -name "*.conf" -exec grep -H "port" {} \;
10. Gutenberg Archive Challenge (From Lab)
I downloaded and extracted the Gutenberg archive:

bash
# Download the archive (using wget or from LMS)
wget http://www.gutenberg.org/cache/epub/feeds/gutenberg.tar.bz2

# Extract the archive
bunzip2 gutenberg.tar.bz2
tar -xvf gutenberg.tar

# Explored the extracted files
ls -la gutenberg/
tree gutenberg/ | head -20

# Searched for specific words
grep -r "verdigris" gutenberg/ | wc -l

# Found the 3rd oldest file
find gutenberg/ -type f -printf "%T+ %p\n" | sort | head -3

# Found the author of a specific file
head -50 gutenberg/1107.txt | grep "Author"

# Found the largest files
du -a gutenberg/ | sort -nr | head -10

⚠️ Errors Faced
Error 1: Permission Denied When Searching Root
Error Message: find: /root: Permission denied

Why it happened: The root directory contains protected system files that regular users cannot access.

How I fixed it: Redirected stderr to /dev/null to hide permission errors:

bash
find / -name "*.conf" 2>/dev/null
What I learned: Redirecting errors makes output cleaner and easier to read.

Error 2: locate Command Not Found
Error Message: command not found: locate

Why it happened: The plocate package was not installed by default.

How I fixed it: Installed it and updated the database:

bash
sudo apt install plocate -y
sudo updatedb
What I learned: Some useful tools need to be installed manually.

🧠 Reflection
What I Learned
find is the most powerful tool for locating files based on name, type, size, and date.

grep is essential for searching inside files for specific text patterns.

locate is much faster than find but requires an updated database.

Combining find and grep with -exec is incredibly powerful for complex searches.

Redirection (2>/dev/null) helps clean up permission error messages.

The grep options (-i, -r, -n, -C, -l, -v, -c) are useful for different search scenarios.

Real-World Application
System administrators use find to locate misconfigured files or large logs consuming disk space.

Security analysts use grep to search logs for malicious activity or error patterns.

Searching is essential for troubleshooting, forensic analysis, and system monitoring.

Archival extraction (like the Gutenberg lab) is common in data processing workflows.
