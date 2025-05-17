# 🧠 Redis Installation & Uninstallation Guide (Ubuntu)

This guide provides instructions to install and fully remove **Redis** on Ubuntu systems using the official Redis APT repository.

---

## 📦 Redis Installation

### 1. Add Redis GPG Key

```bash
curl -fsSL https://packages.redis.io/gpg | sudo gpg --dearmor -o /usr/share/keyrings/redis-archive-keyring.gpg
sudo chmod 644 /usr/share/keyrings/redis-archive-keyring.gpg
```
### 2. Add Redis APT Repository

```bash
echo "deb [signed-by=/usr/share/keyrings/redis-archive-keyring.gpg] https://packages.redis.io/deb $(lsb_release -cs) main" | sudo tee /etc/apt/sources.list.d/redis.list
```

### 3. Update & Install Redis

```bash
sudo apt-get update
sudo apt-get install redis-server
```

### 4. (Optional) Check Available Versions

```bash
apt policy redis-server
```

---

## 🚀 Start and Enable Redis

```bash
sudo systemctl enable redis-server
sudo systemctl start redis-server
systemctl status redis-server.service
```

---

## 🧪 Test Redis CLI

```bash
redis-cli
```

You should see a prompt like:

```
127.0.0.1:6379> ping
```

---

## ❌ Redis Uninstallation

### 1. Shut Down Redis Safely (If Running)

```bash
redis-cli shutdown
```

### 2. Remove Redis Packages and Configuration

```bash
sudo apt remove --purge redis-server redis-tools
sudo apt autoremove
```

### 3. Kill Any Remaining Redis Processes

```bash
ps aux | grep redis
sudo pkill redis
```

---

## ✅ Verification

Check if Redis service is still active:

```bash
systemctl status redis-server
```

Expected output:

```
Unit redis-server.service could not be found.
```

---

