## 🔹 Change SSH Default Port (22 → your custom port)

### 1. Check current listening services

```bash
sudo netstat -tulpn | grep LISTEN
```

(or newer systems use:)

```bash
ss -tulpn | grep LISTEN
```

This shows which ports are already in use.

---

### 2. Edit SSH config

```bash
sudo nano /etc/ssh/sshd_config
```

Find the line:

```ini
#Port 22
```

Uncomment it and change to your custom port, e.g.:

```ini
Port 54321
```

Save and exit (`CTRL+O`, `Enter`, `CTRL+X`).

---

### 3. Adjust firewall rules

First, remove the old SSH rule:

```bash
sudo ufw delete allow 22/tcp
```

Then, allow your new port:

```bash
sudo ufw allow 54321/tcp
```

Reload UFW to apply:

```bash
sudo ufw reload
```

---

### 4. Restart SSH service

```bash
sudo systemctl restart ssh
```

Check status:

```bash
sudo systemctl status ssh
```

---

### 5. Test new connection **before closing old session**

From another terminal (or putty/tab), try:

```bash
ssh -p 54321 user@your_server_ip
```

⚠️ Don’t close your current session until you verify the new port works — otherwise you could lock yourself out.

---

✅ Done — SSH is now running on your custom port.

---

