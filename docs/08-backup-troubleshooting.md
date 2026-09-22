# Module 8: Backups & Troubleshooting

## Objective
Set up a basic automated backup, and document real troubleshooting.

## Prerequisites
- A working web server with content worth backing up (Module 6)

## Steps

### 1. Write a simple backup script
`/usr/local/bin/backup.sh`:
```bash
#!/bin/bash
DATE=$(date +%F)
BACKUP_DIR="/var/backups/website"
mkdir -p "$BACKUP_DIR"
tar -czf "$BACKUP_DIR/site-$DATE.tar.gz" /var/www/lab-site
find "$BACKUP_DIR" -type f -mtime +7 -delete
```
```bash
sudo chmod +x /usr/local/bin/backup.sh
```

### 2. Schedule it with cron
```bash
sudo crontab -e
```
```
0 2 * * * /usr/local/bin/backup.sh
```

### 3. Test it manually and verify
```bash
sudo /usr/local/bin/backup.sh
ls -lh /var/backups/website
```

## Troubleshooting Log
| Issue | Symptom | Root Cause | Fix |
|-------|---------|------------|-----|
| Locked out of SSH | Connection refused after config change | Firewall enabled before allowing new SSH port | Used VirtualBox console to fix ufw rule |
| Forgot login credentials | Couldn't log in after unattended install | Never wrote down username/password | Recovered via GRUB recovery mode (see Module 1) |
| Static IP not applying | ip a showed DHCP address, not static | Duplicate key in netplan YAML | Rewrote file cleanly with tee/heredoc (see Module 1) |

## Key Takeaways
_(fill in — tie back to what this demonstrates for real-world sysadmin work)_
