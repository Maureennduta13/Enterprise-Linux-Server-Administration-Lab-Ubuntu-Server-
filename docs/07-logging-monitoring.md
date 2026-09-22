# Module 7: Logging & Monitoring

## Objective
Learn to read what the system is telling you — logs and basic resource
monitoring — the core of diagnosing problems before they become outages.

## Prerequisites
- A running service to generate logs against (Nginx from Module 6)

## Steps

### 1. Explore systemd journal logs
```bash
journalctl -xe
journalctl -u nginx --since today
journalctl -f
```

### 2. Review application logs
```bash
sudo tail -f /var/log/nginx/access.log
sudo tail -f /var/log/nginx/error.log
```

### 3. Configure log rotation
```bash
cat /etc/logrotate.d/nginx
```

### 4. Basic live monitoring
```bash
sudo apt install -y htop
htop
vmstat 2 5
df -h
free -h
```

## Verification
Generate traffic and confirm it shows up in `access.log` and via `journalctl`.

## Issues & Troubleshooting
_(fill in)_

## Key Takeaways
_(fill in)_
