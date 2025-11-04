# VPS Project Deploy

## Step-1

First enter your VPS IP address

```bash
ssh root@<ip address>
```
👉 Connect to your VPS server via SSH.  
Replace `<ip address>` with your actual server IP.

Then enter your password

```bash
root@<ip address>'s password:
```
👉 Enter your VPS root password when prompted.

---

## Step-2

Go to the `www` folder

```bash
cd /var/www
```
👉 Navigate to the main web directory where your projects are stored.

Check all projects

```bash
ls;
```
👉 List all files and folders in the current directory to confirm existing projects.

---

## Step-3

Clone your GitHub project

```bash
git clone git@github.com:aburayhan-bpi/VPS-Project-Deploy.git
```
👉 Clone your GitHub repository into the VPS server.

---

## Step-4

```bash
ls;
```
👉 Check if the project folder was successfully cloned.

```bash
cd vps-project-deploy/
```
👉 Navigate into your cloned project directory.

```bash
npm i
```
👉 Install all required project dependencies from `package.json`.

```bash
npm run build
```
👉 Build your project for production (compile optimized files).

---

## Step-5

```bash
sudo ufw enable
```
👉 Enable the UFW (Uncomplicated Firewall) on your VPS for security.

```bash
sudo ufw status
```
👉 Check if the firewall is active and see current allowed ports.

```bash
sudo ufw allow 5008
```
👉 Allow traffic through port **5008** (or any port your app uses).

```bash
sudo ufw status
```
👉 Confirm that port 5008 is now open.

```bash
npm run start
```
👉 Start your Node.js application.

```bash
sudo ufw reload
```
👉 Reload the firewall to apply all new rules immediately.

---

## Step-6

```bash
npm i -g pm2
```
👉 Install **PM2** globally — a process manager for Node.js apps.

```bash
pm2 start npm --name "vps-project-deploy" -- start
```
👉 Start your app using PM2, allowing it to run in the background (even after closing the terminal).

```bash
pm2 logs vps-project-deploy
```
👉 View real-time logs of your running application.

```bash
pm2 ls
```
👉 List all running PM2 processes and their status (IDs, uptime, etc.).

---

# After Successfully Deploy — For Redeploy, Follow These Steps

```bash
git pull
```
👉 Fetch and merge the latest code updates from GitHub.

```bash
npm i
```
👉 Reinstall dependencies if there are any new changes.

```bash
npm run build
```
👉 Rebuild your project to apply the new code changes.

```bash
pm2 ls
```
👉 Check your PM2 process list to find the app ID.

```bash
pm2 restart <id_no>
```
👉 Restart your specific app using its PM2 ID to apply the latest version.
