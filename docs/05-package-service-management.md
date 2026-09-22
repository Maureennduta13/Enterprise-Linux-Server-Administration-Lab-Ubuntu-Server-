# Module 5: Package & systemd Service Management

## Objective
Get comfortable managing software and services the way a production Linux
box is actually run — via `apt` and `systemd`, not one-off manual processes.

## Prerequisites
- Base system set up (Module 1)

## Steps

### 1. Package management basics
```bash
sudo apt update
sudo apt list --upgradable
sudo apt install <package>
sudo apt remove <package>
sudo apt autoremove
```

### 2. Explore systemd
```bash
systemctl list-units --type=service --state=running
systemctl status ssh
```

### 3. Create a simple custom systemd service
`/etc/systemd/system/uptime-logger.service`:
```ini
[Unit]
Description=Logs system uptime periodically

[Service]
ExecStart=/usr/local/bin/uptime-logger.sh
Restart=always

[Install]
WantedBy=multi-user.target
```
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now uptime-logger.service
```

## Verification
```bash
systemctl status uptime-logger.service
journalctl -u uptime-logger.service --since "10 min ago"
```

## Issues & Troubleshooting
_(fill in)_

## Key Takeaways
_(fill in)_
