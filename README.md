# 🚀 VPS Project Deployment Guide

A complete **step-by-step guide** to deploy a **Node.js** application on
a fresh **Ubuntu VPS**.

> **Works for:** Ubuntu 22.04 / 24.04, Express.js, Next.js, NestJS and
> most Node.js applications.

------------------------------------------------------------------------

# 📚 Table of Contents

1.  Connect to VPS
2.  Update Server
3.  Install Git
4.  Install Node.js (LTS)
5.  Install pnpm
6.  Install PM2
7.  Clone Repository
8.  Configure Environment Variables
9.  Install Dependencies
10. Build Project
11. Configure Firewall
12. Run Application
13. Run with PM2
14. Auto Start After Reboot
15. Configure Nginx
16. Install SSL
17. Configure Domain
18. Redeploy
19. Useful Commands
20. Troubleshooting

------------------------------------------------------------------------

# 1. Connect to VPS

``` bash
ssh root@<your-vps-ip>
```

------------------------------------------------------------------------

# 2. Update Server

``` bash
sudo apt update && sudo apt upgrade -y
```

------------------------------------------------------------------------

# 3. Install Git

``` bash
sudo apt install git -y
git --version
```

------------------------------------------------------------------------

# 4. Install Node.js (22 LTS)

``` bash
curl -fsSL https://deb.nodesource.com/setup_22.x | sudo -E bash -

sudo apt install -y nodejs

node -v
npm -v
```

------------------------------------------------------------------------

# 5. Install pnpm (Optional)

Recommended:

``` bash
sudo corepack enable
sudo corepack prepare pnpm@latest --activate
pnpm -v
```

Alternative:

``` bash
sudo npm install -g pnpm
```

------------------------------------------------------------------------

# 6. Install PM2

``` bash
sudo npm install -g pm2
pm2 -v
```

------------------------------------------------------------------------

# 7. Clone Repository

``` bash
cd /var/www

git clone git@github.com:<username>/<repository>.git

cd <repository>
```

------------------------------------------------------------------------

# 8. Configure Environment Variables

``` bash
cp .env.example .env

nano .env
```

Save:

    CTRL + X
    Y
    ENTER

------------------------------------------------------------------------

# 9. Install Dependencies

### npm

``` bash
npm install
```

### pnpm

``` bash
pnpm install
```

------------------------------------------------------------------------

# 10. Build Project

``` bash
npm run build
```

or

``` bash
pnpm build
```

------------------------------------------------------------------------

# 11. Configure Firewall

``` bash
sudo ufw enable

sudo ufw allow OpenSSH

sudo ufw allow 5008

sudo ufw reload

sudo ufw status
```

Replace **5008** with your application port.

------------------------------------------------------------------------

# 12. Test Application

``` bash
npm run start
```

or

``` bash
pnpm start
```

Press **CTRL + C** to stop.

------------------------------------------------------------------------

# 13. Run with PM2

### npm

``` bash
pm2 start npm --name "<project-name>" -- start
```

### pnpm

``` bash
pm2 start "pnpm start" --name "<project-name>"
```

Useful commands

``` bash
pm2 ls
pm2 logs <project-name>
pm2 restart <project-name>
pm2 stop <project-name>
pm2 delete <project-name>
pm2 monit
```

------------------------------------------------------------------------

# 14. Auto Start After Reboot

``` bash
pm2 startup
```

Run the generated command, then:

``` bash
pm2 save
```

------------------------------------------------------------------------

# 15. Configure Nginx

Install:

``` bash
sudo apt install nginx -y
```

Create config:

``` bash
sudo nano /etc/nginx/sites-available/<project-name>
```

Example:

``` nginx
server {
    listen 80;

    server_name example.com www.example.com;

    location / {
        proxy_pass http://localhost:5008;

        proxy_http_version 1.1;

        proxy_set_header Upgrade $http_upgrade;
        proxy_set_header Connection "upgrade";

        proxy_set_header Host $host;
        proxy_set_header X-Real-IP $remote_addr;
        proxy_set_header X-Forwarded-For $proxy_add_x_forwarded_for;
        proxy_set_header X-Forwarded-Proto $scheme;
    }
}
```

Enable:

``` bash
sudo ln -s /etc/nginx/sites-available/<project-name> /etc/nginx/sites-enabled/

sudo nginx -t

sudo systemctl reload nginx
```

------------------------------------------------------------------------

# 16. Install SSL

``` bash
sudo apt install certbot python3-certbot-nginx -y

sudo certbot --nginx

sudo certbot renew --dry-run
```

------------------------------------------------------------------------

# 17. Configure Domain

Create DNS records

  Type   Name   Value
  ------ ------ --------
  A      @      VPS IP
  A      www    VPS IP

Allow HTTP/HTTPS

``` bash
sudo ufw allow 'Nginx Full'
```

or

``` bash
sudo ufw allow 80
sudo ufw allow 443
```

------------------------------------------------------------------------

# 18. Redeploy

``` bash
cd /var/www/<repository>

git pull

npm install

npm run build

pm2 restart <project-name>
```

or

``` bash
cd /var/www/<repository>

pnpm install

pnpm build

pm2 restart <project-name>
```

------------------------------------------------------------------------

# 19. Useful Commands

## PM2

``` bash
pm2 ls
pm2 logs
pm2 restart all
pm2 stop all
pm2 delete all
pm2 save
```

## Git

``` bash
git status
git branch
git pull
git log --oneline
```

## Server

``` bash
pwd
df -h
free -h
htop
sudo ss -tulpn
```

## Logs

``` bash
pm2 logs

sudo tail -f /var/log/nginx/access.log

sudo tail -f /var/log/nginx/error.log
```

------------------------------------------------------------------------

# 20. Troubleshooting

### Port already in use

``` bash
sudo lsof -i :5008
sudo kill -9 <PID>
```

### PM2 not starting

``` bash
pm2 logs
pm2 restart <project-name>
```

### Nginx issue

``` bash
sudo nginx -t
sudo systemctl restart nginx
```

### Firewall

``` bash
sudo ufw status
sudo ufw reload
```

------------------------------------------------------------------------

# ✅ Production Checklist

-   [ ] Ubuntu updated
-   [ ] Git installed
-   [ ] Node.js LTS installed
-   [ ] pnpm installed (optional)
-   [ ] PM2 installed
-   [ ] Environment variables configured
-   [ ] Application builds successfully
-   [ ] Firewall configured
-   [ ] PM2 running
-   [ ] Nginx configured
-   [ ] SSL enabled
-   [ ] Domain connected
-   [ ] PM2 saved for reboot

------------------------------------------------------------------------

Happy Deploying! 🚀
