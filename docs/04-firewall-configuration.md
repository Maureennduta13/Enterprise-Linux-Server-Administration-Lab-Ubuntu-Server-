# Module 4: Firewall Configuration (UFW)

## Objective
Lock the server down to only the ports it actually needs to expose.

## Prerequisites
- SSH hardened and confirmed working on its new port (Module 3)

## Steps

### 1. Set default policies
```bash
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

### 2. Allow only the ports you need
```bash
sudo ufw allow 2222/tcp   # SSH (custom port from Module 3)
sudo ufw allow 80/tcp     # HTTP (for Module 6 web server)
sudo ufw allow 443/tcp    # HTTPS
```

### 3. Enable the firewall
```bash
sudo ufw enable
```

### 4. Check status
```bash
sudo ufw status verbose
```

## Verification
Confirm SSH still works from the host, and that a port you didn't open is refused.

## Issues & Troubleshooting
_(fill in — e.g. locking yourself out by enabling ufw before allowing the SSH port)_

## Key Takeaways
_(fill in)_
