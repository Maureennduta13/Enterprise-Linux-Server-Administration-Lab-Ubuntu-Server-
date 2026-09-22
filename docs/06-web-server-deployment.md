# Module 6: Web Server Deployment (Nginx)

## Objective
Deploy and configure a production-style web server.

## Prerequisites
- Firewall allows port 80/443 (Module 4)

## Steps

### 1. Install Nginx
```bash
sudo apt install -y nginx
sudo systemctl enable --now nginx
```

### 2. Deploy a basic site
```bash
sudo nano /var/www/html/index.html
```

### 3. Create a custom server block
`/etc/nginx/sites-available/lab-site`:
```nginx
server {
    listen 80;
    server_name lab.local;
    root /var/www/lab-site;
    index index.html;
}
```
```bash
sudo ln -s /etc/nginx/sites-available/lab-site /etc/nginx/sites-enabled/
sudo nginx -t
sudo systemctl reload nginx
```

## Verification
```powershell
curl http://192.168.56.10
```

## Issues & Troubleshooting
_(fill in)_

## Key Takeaways
_(fill in)_
