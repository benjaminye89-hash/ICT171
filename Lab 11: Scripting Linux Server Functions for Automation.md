# Lab 11: Scripting Linux Server Functions for Automation

**Date:** 15/07/2026  
**Duration:** 2 hours  
**Status:** Complete  

---

## 🎯 Objective
To write advanced Bash scripts for automating Linux server administration tasks, including system updates, backups, monitoring, and log rotation, and to schedule these scripts using cron jobs.

---

## 🛠️ Tools Used
- Ubuntu 24.04 LTS (VirtualBox VM)
- Bash scripting
- Cron (scheduling)
- Systemd (for service management)

---

## 📋 Step-by-Step Process

### 1. Created an Automated System Update Script
I wrote a script to automate system updates and upgrades:

```bash
#!/bin/bash
# auto_update.sh - Automated system update script

LOG_FILE="/var/log/auto_update.log"
DATE=$(date "+%Y-%m-%d %H:%M:%S")

echo "[$DATE] Starting system update..." | tee -a $LOG_FILE

# Update package lists
sudo apt update 2>&1 | tee -a $LOG_FILE

# Upgrade packages (non-interactive)
sudo apt upgrade -y 2>&1 | tee -a $LOG_FILE

# Remove unnecessary packages
sudo apt autoremove -y 2>&1 | tee -a $LOG_FILE

# Check if reboot is required
if [ -f /var/run/reboot-required ]; then
    echo "[$DATE] ⚠️ Reboot required!" | tee -a $LOG_FILE
else
    echo "[$DATE] ✅ Update completed successfully" | tee -a $LOG_FILE
fi
2. Created a System Health Monitoring Script
I wrote a script to monitor system health and send alerts:

bash
#!/bin/bash
# system_health.sh - System health monitoring script

ALERT_EMAIL="benjamin.ye@murdoch.edu.au"
LOG_FILE="/var/log/system_health.log"
DATE=$(date "+%Y-%m-%d %H:%M:%S")

echo "[$DATE] Checking system health..." >> $LOG_FILE

# Check disk usage (alert if > 80%)
DISK_USAGE=$(df -h / | awk 'NR==2 {print $5}' | sed 's/%//')
if [ $DISK_USAGE -gt 80 ]; then
    echo "[$DATE] ⚠️ WARNING: Disk usage is at ${DISK_USAGE}%" >> $LOG_FILE
    echo "Disk usage alert: ${DISK_USAGE}%" | mail -s "Alert: Disk Usage" $ALERT_EMAIL
else
    echo "[$DATE] ✅ Disk usage is at ${DISK_USAGE}%" >> $LOG_FILE
fi

# Check memory usage
MEMORY_USAGE=$(free | grep Mem | awk '{print ($3/$2) * 100.0}')
if (( $(echo "$MEMORY_USAGE > 90" | bc -l) )); then
    echo "[$DATE] ⚠️ WARNING: Memory usage is at ${MEMORY_USAGE}%" >> $LOG_FILE
fi

# Check running services
SERVICES=("apache2" "mysql" "ssh")
for SERVICE in "${SERVICES[@]}"; do
    if systemctl is-active --quiet $SERVICE; then
        echo "[$DATE] ✅ $SERVICE is running" >> $LOG_FILE
    else
        echo "[$DATE] ❌ $SERVICE is NOT running!" >> $LOG_FILE
        sudo systemctl start $SERVICE
    fi
done

echo "[$DATE] Health check completed." >> $LOG_FILE
3. Created a Log Rotation Script
I wrote a script to rotate and compress log files:

bash
#!/bin/bash
# log_rotate.sh - Log rotation script

LOG_DIR="/var/log"
BACKUP_DIR="/var/log/archive"
DATE=$(date "+%Y%m%d")

# Create archive directory if it doesn't exist
mkdir -p $BACKUP_DIR

# Find and compress logs older than 7 days
find $LOG_DIR -name "*.log" -type f -mtime +7 -exec gzip {} \;

# Move compressed logs to archive
find $LOG_DIR -name "*.gz" -type f -mtime +7 -exec mv {} $BACKUP_DIR/ \;

# Delete logs older than 30 days
find $BACKUP_DIR -name "*.gz" -type f -mtime +30 -exec rm {} \;

echo "$(date): Log rotation completed" >> /var/log/log_rotate.log
4. Created a Backup and Restore Script
I wrote a comprehensive backup script:

bash
#!/bin/bash
# backup_restore.sh - Automated backup with restore option

BACKUP_DIR="/backup"
SOURCE_DIRS=("/home/benji/Documents" "/etc" "/var/www")
DATE=$(date "+%Y%m%d_%H%M%S")
LOG_FILE="/var/log/backup.log"

# Function to create backup
create_backup() {
    mkdir -p $BACKUP_DIR/$DATE
    echo "$(date): Starting backup..." >> $LOG_FILE
    
    for SOURCE in "${SOURCE_DIRS[@]}"; do
        if [ -d "$SOURCE" ]; then
            BASENAME=$(basename $SOURCE)
            tar -czf "$BACKUP_DIR/$DATE/${BASENAME}_$DATE.tar.gz" "$SOURCE" 2>/dev/null
            echo "$(date): Backed up $SOURCE" >> $LOG_FILE
        else
            echo "$(date): ⚠️ $SOURCE does not exist" >> $LOG_FILE
        fi
    done
    
    echo "$(date): ✅ Backup completed" >> $LOG_FILE
}

# Function to list backups
list_backups() {
    echo "📁 Available backups:"
    ls -lh $BACKUP_DIR
}

# Function to restore a backup
restore_backup() {
    if [ -z "$1" ]; then
        echo "Usage: $0 restore <filename>"
        return 1
    fi
    
    BACKUP_FILE="$BACKUP_DIR/$1"
    if [ -f "$BACKUP_FILE" ]; then
        echo "$(date): Starting restore from $1..." >> $LOG_FILE
        tar -xzf "$BACKUP_FILE" -C / 2>/dev/null
        echo "$(date): ✅ Restore completed" >> $LOG_FILE
    else
        echo "❌ Backup file $1 not found"
    fi
}

# Main execution
case "$1" in
    backup)
        create_backup
        ;;
    list)
        list_backups
        ;;
    restore)
        restore_backup "$2"
        ;;
    *)
        echo "Usage: $0 {backup|list|restore <filename>}"
        exit 1
        ;;
esac
5. Scheduled Scripts with Cron
I scheduled all scripts to run automatically:

bash
# Edit crontab
crontab -e

# Added these entries:
# System update every Sunday at 2:00 AM
0 2 * * 0 /home/benji/scripts/auto_update.sh

# System health check every hour
0 * * * * /home/benji/scripts/system_health.sh

# Log rotation every day at 3:00 AM
0 3 * * * /home/benji/scripts/log_rotate.sh

# Full backup every day at 4:00 AM
0 4 * * * /home/benji/scripts/backup_restore.sh backup
6. Created a Service Management Script
I wrote a script to manage multiple services:

bash
#!/bin/bash
# service_manager.sh - Manage multiple services

SERVICES=("apache2" "mysql" "ssh" "cron")

# Function to start all services
start_all() {
    for SERVICE in "${SERVICES[@]}"; do
        sudo systemctl start $SERVICE
        echo "Started $SERVICE"
    done
}

# Function to stop all services
stop_all() {
    for SERVICE in "${SERVICES[@]}"; do
        sudo systemctl stop $SERVICE
        echo "Stopped $SERVICE"
    done
}

# Function to check all services
status_all() {
    for SERVICE in "${SERVICES[@]}"; do
        if systemctl is-active --quiet $SERVICE; then
            echo "✅ $SERVICE is running"
        else
            echo "❌ $SERVICE is not running"
        fi
    done
}

# Main menu
case "$1" in
    start)
        start_all
        ;;
    stop)
        stop_all
        ;;
    status)
        status_all
        ;;
    restart)
        stop_all
        sleep 2
        start_all
        ;;
    *)
        echo "Usage: $0 {start|stop|restart|status}"
        exit 1
        ;;
esac
7. Created a Web Server Setup Script
I wrote a script to quickly set up a web server:

bash
#!/bin/bash
# setup_webserver.sh - Quick web server setup

# Check if running as root
if [ "$EUID" -ne 0 ]; then 
    echo "Please run as root: sudo $0"
    exit 1
fi

# Variables
DOMAIN=${1:-"localhost"}

echo "🚀 Setting up web server for $DOMAIN..."

# Update system
apt update && apt upgrade -y

# Install Apache and tools
apt install apache2 -y
apt install php libapache2-mod-php php-mysql -y
apt install certbot python3-certbot-apache -y

# Enable modules
a2enmod rewrite
a2enmod ssl

# Create website directory
mkdir -p /var/www/$DOMAIN/public_html
mkdir -p /var/www/$DOMAIN/logs

# Set permissions
chown -R www-data:www-data /var/www/$DOMAIN
chmod -R 755 /var/www/$DOMAIN

# Create sample index.html
cat > /var/www/$DOMAIN/public_html/index.html << EOF
<!DOCTYPE html>
<html>
<head><title>Welcome to $DOMAIN</title></head>
<body>
<h1>🚀 Server is running!</h1>
<p>Hostname: $(hostname)</p>
<p>IP: $(hostname -I | awk '{print $1}')</p>
<p>Date: $(date)</p>
</body>
</html>
EOF

# Create virtual host configuration
cat > /etc/apache2/sites-available/$DOMAIN.conf << EOF
<VirtualHost *:80>
    ServerAdmin admin@$DOMAIN
    ServerName $DOMAIN
    ServerAlias www.$DOMAIN
    DocumentRoot /var/www/$DOMAIN/public_html
    ErrorLog /var/www/$DOMAIN/logs/error.log
    CustomLog /var/www/$DOMAIN/logs/access.log combined
</VirtualHost>
EOF

# Enable the site
a2ensite $DOMAIN.conf
systemctl restart apache2

echo "✅ Web server setup complete!"
echo "Visit: http://$DOMAIN"
8. Verified Cron Job Execution
I verified the cron jobs were running:

bash
# List cron jobs
crontab -l

# Check cron logs
sudo tail -f /var/log/syslog | grep CRON

# Check script logs
cat /var/log/auto_update.log
cat /var/log/system_health.log
