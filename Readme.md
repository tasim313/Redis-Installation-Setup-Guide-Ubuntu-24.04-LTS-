# 🚀 Redis Installation & Setup Guide (Ubuntu 24.04 LTS)

This guide provides a **clean, production-ready setup** for Redis (Community Edition / Stack 7.x) on Ubuntu.

It follows **official Redis repository standards** to ensure:
- Stability
- Latest supported versions
- Proper service management

---

## 📌 Table of Contents

- [Prerequisites](#-prerequisites)
- [Installation (Official Redis Repo)](#-installation-official-redis-repo)
- [Service Management](#-service-management)
- [Verify Installation](#-verify-installation)
- [Basic Configuration](#-basic-configuration)
- [Remote Access Setup](#-remote-access-setup)
- [Security Hardening](#-security-hardening)
- [Performance Tuning](#-performance-tuning)
- [Troubleshooting](#-troubleshooting)

---

## 🧱 Prerequisites

- Ubuntu 24.04 LTS (Noble)
- Sudo privileges
- Internet connection

---

## ⚙️ Installation (Official Redis Repository)

### Step 1: Install dependencies

```bash
sudo apt-get update
sudo apt-get install -y lsb-release curl gpg
```
### Step 2: Add Redis GPG key
```bash
curl -fsSL https://packages.redis.io/gpg | \
sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg

sudo chmod 644 /usr/share/keyrings/redis-archive-keyring.gpg
```
### Step 3: Add Redis repository
```bash
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] \
https://packages.redis.io/deb $(lsb_release -cs) main" | \
sudo tee /etc/apt/sources.list.d/redis.list
```
### Step 4: Install Redis
```bash
sudo apt-get update
sudo apt-get install -y redis
```
### 🔄 Service Management
Redis usually starts automatically
check status
```bash
sudo systemctl status redis-server
```

Start Redis (if needed)
```bash
sudo systemctl start redis-server
```
Restart Redis
```bash
sudo systemctl restart redis-server
```
✅ Verify Installation
Connect using CLI:
```bash
redis-cli
```
Test:
```bash
ping
```
Expected output:
```bash
PONG
```
⚙️ Basic Configuration
Edit Redis config:
```bash
sudo nano /etc/redis/redis.conf
```

Key settings
Bind address (default local only)
```bash
bind 127.0.0.1
```
Port
```bash
port 6379
```
Supervised mode (recommended for systemd)
```bash
supervised systemd
```
🌐 Remote Access Setup (Optional)
Step 1: Allow external connections

Edit:
```bash
sudo nano /etc/redis/redis.conf
```
Change:
```bash
bind 0.0.0.0
```
### Step 2: Restart Redis
```bash
sudo systemctl restart redis-server
```
### Step 3: Open firewall
```bash
sudo ufw allow 6379/tcp
```
⚠️ Restrict IP access in production.

🔐 Security Hardening
Set Redis password

Edit config:
```bash
sudo nano /etc/redis/redis.conf
```
Find:
```bash
# requirepass foobared
```
Change to:
```bash
requirepass StrongRedisPassword
```
Restart:
```bash
sudo systemctl restart redis-server
```
Connect with password
```bash
redis-cli
```
```bash
AUTH StrongRedisPassword
```
⚡ Performance Tuning
Edit:
```bash
sudo nano /etc/redis/redis.conf
```
Recommended baseline
```bash
maxmemory 512mb
maxmemory-policy allkeys-lru
```
Persistence (AOF recommended)
```bash
appendonly yes
```
🛠️ Troubleshooting
Check logs
```bash
sudo journalctl -u redis-server
```
Check port
```bash
sudo ss -plnt | grep 6379
```
Restart service
```bash
sudo systemctl restart redis-server
```
⚠️ Common Pitfalls
- Redis not restarted after config change
- Port 6379 blocked by firewall
- No password set in production
- Binding to 0.0.0.0 without security
- Memory limits not configured
