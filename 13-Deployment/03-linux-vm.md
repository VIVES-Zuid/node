# 🖥️ Deploying on a Linux VM

## Chapter 13: Own Server on the Cloud

---

## ☁️ Cloud Providers with Free VMs

| Provider | Free VM | RAM | Notes |
|----------|---------|-----|-------|
| **Oracle Cloud** | 2 VMs **always free** | 1 GB each | Best free tier, ARM |
| **Google Cloud** | e2-micro (1 region) | 0.6 GB | 30 GB disk |
| **Azure** | B1s (12 months) | 1 GB | Student credits available |
| **AWS** | t2.micro (12 months) | 1 GB | 750 hours/month |

> 💡 **Recommendation for students**: Oracle Cloud Always Free — no expiry, no credit card tricks.

---

## 🚀 Step 1: Create Your VM

### Oracle Cloud (Always Free)

1. Mail your lecturer for an Academy invite. Do not sign up on your own because you'll need a credit card for it. 
2. Go to https://cloud.oracle.com and then **Compute → Instances → Create Instance**
3. Choose **Ubuntu 22.04 or 24.04** (Canonical)
4. Shape: **VM.Standard.A1.Flex** (ARM, always free)
5. Add your **SSH public key**
6. Click **Create**

### Get your SSH key (if you don't have one)

```bash
ssh-keygen -t ed25519 -C "your@email.com"
cat ~/.ssh/id_ed25519.pub   # copy this to Oracle
```

⚠️ Note: ssh-keygen also works on windows but not cat. You'll have to open the key in notepad to read out the content. 

```cmd
REM Windows cmd

REM to copy the contents of your key. 
type %USERPROFILE%\.ssh\id_ed25519.pub
REM to directly copy the contents to the clipboard.
type %USERPROFILE%\.ssh\id_rsa.pub | clip
```

```powershell
# Windows Powershell

# Display
cat ~/.ssh/id_rsa.pub

# Copy to clipboard
Get-Content ~/.ssh/id_rsa.pub | Set-Clipboard
```

---

## 🔌 Step 2: Connect via SSH

```bash
# Oracle Cloud uses 'ubuntu' as the default user
ssh ubuntu@<YOUR_VM_IP>

# GCP uses your Google username
ssh <your-username>@<VM_IP>

# Or use the cloud shell in the browser
```

---

## 📦 Step 3: Install Node.js (via nvm)

```bash
# Install nvm (Node Version Manager)
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.39.7/install.sh | bash

# Reload shell
source ~/.bashrc

# Install Node.js LTS
nvm install --lts
nvm use --lts

# Verify
node --version
npm --version
```

> **Why nvm?** Easy to upgrade Node.js versions, no `sudo` needed for npm.

---

## 📁 Step 4: Deploy Your Code

### Option A: Clone from GitHub

```bash
git clone https://github.com/your-username/your-api.git
cd your-api
npm install
```

⚠️ If your github repository is private you will have to create ssh keys on the server too and copy the public key to your Github Account. 
Also see "Get your SSH key (if you don't have one)" here above and https://docs.github.com/en/authentication/connecting-to-github-with-ssh/adding-a-new-ssh-key-to-your-github-account 

### Option B: Copy files with `scp`

```bash
# From your local machine
scp -r ./my-api ubuntu@<VM_IP>:/home/ubuntu/
```

---

## ⚙️ Step 5: Configure Environment Variables

```bash
# Create .env file on the server
nano .env
```

```bash
PORT=3000
MONGODB_URI=mongodb+srv://user:pass@cluster.mongodb.net/mydb
JWT_SECRET=super_secret_production_key
NODE_ENV=production
```

```bash
# Test it runs
npm start
# Ctrl+C to stop
```

---

## 🔥 Step 6: Open the Firewall

### Oracle Cloud — Ingress Rules

1. Go to **Networking → Virtual Cloud Networks**
2. Click your VCN → **Security Lists**
3. Add **Ingress Rule**:
   - Source: `0.0.0.0/0`
   - Port: `80, 443, 3000`

### Ubuntu firewall (ufw - OPTIONAL)

```bash
sudo ufw allow OpenSSH
sudo ufw allow 3000
sudo ufw enable
```

---

## 🌐 Step 7: Test Your API

```bash
# From your local machine
curl http://<VM_IP>:3000/health

# Or open in browser
# http://<VM_IP>:3000
```

---

## ⚠️ Problem: SSH Disconnect Kills Your Server

```bash
# This dies when you close the terminal!
node index.js
```

> **Next**: PM2 and Nginx solve this problem permanently.

---

[← Production Prep](./02-production-prep.md) | [🏠 Home](../README.md) | [Next: PM2 & Nginx →](./04-pm2-nginx.md)
