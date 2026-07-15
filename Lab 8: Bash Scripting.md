# Lab 8: Bash Scripting

**Date:** 15/07/2026  
**Duration:** 2 hours  
**Status:** Complete  

---

## 🎯 Objective
To write and execute Bash shell scripts for automating server tasks, including conditional logic, loops, and scheduling with cron jobs.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- Bash (Bourne Again SHell)
- `chmod`, `cron`, `nano`/`vim`

---

## 📋 Step-by-Step Process

### 1. Created a Basic Bash Script
I created my first Bash script called `hello.sh`:

```bash
#!/bin/bash
# This is my first Bash script
echo "Hello, World!"
echo "Today is $(date)"
2. Made the Script Executable
I changed the permissions to make the script executable:

bash
chmod +x hello.sh
3. Ran the Script
I executed the script:

bash
./hello.sh
Output:

text
Hello, World!
Today is Wed Jul 15 14:30:00 +08 2026
4. Created a Backup Script
I wrote a script to automate backing up my Documents folder:

bash
#!/bin/bash
# backup.sh - Backup script for Documents folder

# Variables
SOURCE_DIR="$HOME/Documents"
BACKUP_DIR="$HOME/backup"
DATE=$(date +"%Y%m%d_%H%M%S")
BACKUP_FILE="backup_$DATE.tar.gz"

# Create backup directory if it doesn't exist
mkdir -p "$BACKUP_DIR"

# Create the backup archive
tar -czf "$BACKUP_DIR/$BACKUP_FILE" "$SOURCE_DIR" 2>/dev/null

# Check if backup was successful
if [ $? -eq 0 ]; then
    echo "✅ Backup successful: $BACKUP_FILE"
    echo "Size: $(du -h "$BACKUP_DIR/$BACKUP_FILE" | cut -f1)"
else
    echo "❌ Backup failed!"
    exit 1
fi

# List all backups
echo ""
echo "📁 All backups:"
ls -lh "$BACKUP_DIR" | grep ".tar.gz"
5. Used Variables and User Input
I created a script that takes user input:

bash
#!/bin/bash
# greet.sh - Simple greeting script

echo "What is your name?"
read name

echo "Hello, $name! Welcome to Bash scripting."
echo "Today is $(date +%A, %B %d, %Y)"
6. Used Conditional Statements (if/else)
I wrote a script to check if a service is running:

bash
#!/bin/bash
# check_service.sh - Check if Apache is running

SERVICE="apache2"

if systemctl is-active --quiet $SERVICE; then
    echo "✅ $SERVICE is running"
else
    echo "❌ $SERVICE is not running"
    echo "Starting $SERVICE..."
    sudo systemctl start $SERVICE
    echo "✅ $SERVICE started successfully"
fi
7. Used Loops (for and while)
I created scripts with loops:

For Loop Example:

bash
#!/bin/bash
# create_files.sh - Create multiple files

for i in {1..10}; do
    echo "Creating file file_$i.txt"
    touch "file_$i.txt"
done

echo "✅ Created $(ls -1 file_*.txt | wc -l) files"
While Loop Example:

bash
#!/bin/bash
# countdown.sh - Simple countdown timer

count=5

while [ $count -gt 0 ]; do
    echo "⏳ $count seconds remaining..."
    sleep 1
    count=$((count - 1))
done

echo "🚀 Countdown complete!"
8. Used Functions
I wrote a script with reusable functions:

bash
#!/bin/bash
# server_tools.sh - Server management functions

# Function to check system memory
check_memory() {
    echo "📊 Memory Usage:"
    free -h
    echo ""
}

# Function to check disk space
check_disk() {
    echo "💾 Disk Space:"
    df -h /
    echo ""
}

# Function to list running services
list_services() {
    echo "🔄 Running Services:"
    systemctl list-units --type=service --state=running | head -10
    echo ""
}

# Main execution
echo "=== Server Tools ==="
echo "1. Check Memory"
echo "2. Check Disk"
echo "3. List Services"
echo ""

case $1 in
    1) check_memory ;;
    2) check_disk ;;
    3) list_services ;;
    *) echo "Usage: ./server_tools.sh [1|2|3]" ;;
esac
9. Scheduled Script with Cron
I scheduled the backup script to run automatically using cron:

bash
# Open crontab editor
crontab -e

# Added this line to run daily at 2:00 AM
0 2 * * * /home/benji/backup.sh >> /var/log/backup.log 2>&1
Cron Schedule Syntax:

text
* * * * * command_to_run
│ │ │ │ │
│ │ │ │ └─── Day of week (0-7, 0=Sun)
│ │ │ └───── Month (1-12)
│ │ └─────── Day of month (1-31)
│ └───────── Hour (0-23)
└─────────── Minute (0-59)
10. Logged Output to a File
I modified scripts to log output:

bash
#!/bin/bash
# backup_with_log.sh - Backup with logging

LOG_FILE="/var/log/backup.log"
BACKUP_DIR="$HOME/backup"

# Create log directory if it doesn't exist
sudo mkdir -p /var/log
sudo touch $LOG_FILE
sudo chmod 666 $LOG_FILE

echo "$(date): Starting backup" >> $LOG_FILE
tar -czf "$BACKUP_DIR/backup_$(date +%Y%m%d).tar.gz" "$HOME/Documents" 2>/dev/null

if [ $? -eq 0 ]; then
    echo "$(date): ✅ Backup successful" >> $LOG_FILE
else
    echo "$(date): ❌ Backup failed" >> $LOG_FILE
fi


⚠️ Errors Faced
Error 1: Permission Denied When Running Script
Error Message: bash: ./hello.sh: Permission denied

Why it happened: The script did not have execute permissions.

How I fixed it: Added execute permission:

bash
chmod +x hello.sh
What I learned: Scripts need execute permission (chmod +x) to run.

Error 2: Command Not Found (Shebang Issue)
Error Message: ./script.sh: line 1: #!/bin/bash: No such file or directory

Why it happened: The shebang (#!/bin/bash) was not on the first line or had extra characters.

How I fixed it: Ensured the first line was exactly #!/bin/bash with no spaces.

What I learned: The shebang must be the first line of a Bash script.

Error 3: Variable Not Expanding Correctly
Error Message: (No error, but variable showed as literal text)

Why it happened: I used single quotes instead of double quotes around variables.

How I fixed it: Changed '$SOURCE_DIR' to "$SOURCE_DIR".

What I learned: Variables need double quotes to expand properly.

Error 4: Cron Job Not Running
Error Message: (Cron job didn't execute)

Why it happened: The script path was relative, and cron uses absolute paths.

How I fixed it: Used full path in cron:

bash
0 2 * * * /home/benji/backup.sh >> /var/log/backup.log 2>&1
What I learned: Cron requires absolute paths for scripts and commands.

🧠 Reflection
What I Learned
Bash scripts are powerful tools for automating repetitive tasks.

Variables store values and should be referenced with $variable.

Conditional statements (if/else) allow scripts to make decisions.

Loops (for, while) enable processing multiple items.

Functions make scripts modular and reusable.

Cron jobs automate scripts to run at scheduled times.

Logging helps track script execution and debug issues.

Key Commands Summary
Command	Purpose
#!/bin/bash	Shebang – defines the interpreter
chmod +x script.sh	Make script executable
./script.sh	Execute script
bash script.sh	Execute script (without execute permission)
read variable	Read user input
if [ condition ]; then ... fi	Conditional statement
for i in {1..10}; do ... done	For loop
while [ condition ]; do ... done	While loop
function_name() { ... }	Define a function
crontab -e	Edit cron jobs
crontab -l	List cron jobs
Real-World Application
System administrators use Bash scripts to automate backups, updates, and monitoring.

DevOps engineers use scripts to deploy applications and manage infrastructure.

Cron jobs are essential for scheduled maintenance and reporting.

Scripting reduces human error and saves time in repetitive tasks.
