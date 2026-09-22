# Module 3: SSH Hardening

## Objective
Move from password-based root SSH access (the default, insecure state) to
key-based authentication with root login disabled — standard practice for
any internet-facing Linux server.

## Prerequisites
- SSH server installed and reachable (Module 1)
- A non-root user with sudo access (Module 2)

## Steps

### 1. Generate an SSH key pair on the host (Windows)
```powershell
ssh-keygen -t ed25519 -C "lab-key"
```

### 2. Copy the public key to the VM
```bash
ssh-copy-id maureenserver@192.168.56.10
```

### 3. Edit SSH daemon config
```bash
sudo nano /etc/ssh/sshd_config
```
```
PermitRootLogin no
PasswordAuthentication no
Port 2222
```

### 4. Restart SSH and test in a NEW terminal before closing the old one
```bash
sudo systemctl restart ssh
```

### 5. Install fail2ban to block brute-force attempts
```bash
sudo apt install -y fail2ban
sudo systemctl enable --now fail2ban
```

## Verification
```powershell
ssh -p 2222 maureenserver@192.168.56.10
```

## Issues & Troubleshooting
_(fill in once completed — this step is the most likely to lock you out if
done wrong; document what you'd do to recover using the VirtualBox console)_

## Key Takeaways
_(fill in)_
