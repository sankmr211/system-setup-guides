# ✅ Stop Unnecessary Services + Enable Audit Logging
1. **Stop unnecessary services** (free RAM + CPU)
2. **Don’t break Desktop OS**
3. Enable **audit logging with timestamp** (for tracking who did what + when)


To check **unwanted services running** using `systemctl`, you can use these commands 👇

## 1. Check the list of the Service
---

### 1. List all running services

```bash
systemctl list-units --type=service --state=running
```

This shows only services currently running.

---

### 2. Check all enabled services (auto-start on boot)

```bash
systemctl list-unit-files --type=service --state=enabled
```

If any unwanted service is enabled, it will start automatically after reboot.

---

## 2. Manage the services


### 1. If you don’t use printers

```bash
sudo systemctl stop cups.service cups-browsed.service
sudo systemctl disable cups.service cups-browsed.service
```

🟢 Frees memory, printer system gone.

---

### 2. If you don’t use Bluetooth

```bash
sudo systemctl stop bluetooth.service
sudo systemctl disable bluetooth.service
```

---

### 3. If you don’t use AnyDesk

```bash
sudo systemctl stop anydesk.service
sudo systemctl disable anydesk.service
```

---

### 4. If you don’t use Avahi (local network discovery)

```bash
sudo systemctl stop avahi-daemon.service
sudo systemctl disable avahi-daemon.service
```

Avahi is only for LAN discovery like printers / local hostname `.local`.

---

### 5. If you don’t use Modem / SIM / Dongle

```bash
sudo systemctl stop ModemManager.service
sudo systemctl disable ModemManager.service
```

---

### 6. If you don’t need fingerprint login

```bash
sudo systemctl stop fprintd.service
sudo systemctl disable fprintd.service
```

---

### 7. If you don’t care about crash reporting

```bash
sudo systemctl stop whoopsie.service
sudo systemctl disable whoopsie.service
sudo systemctl stop kerneloops.service
sudo systemctl disable kerneloops.service
```

---

### 8. If you don’t use firmware update daemon

```bash
sudo systemctl stop fwupd.service
sudo systemctl disable fwupd.service
```

---

# ⚠️ Services you SHOULD NOT stop (desktop will break)

❌ Don’t stop these:

* `gdm.service` (GUI login screen)
* `NetworkManager.service` (network will die)
* `dbus.service` (system communication will break)
* `systemd-logind.service` (login sessions)
* `systemd-udevd.service` (device detection)
* `systemd-journald.service` (logs)
* `systemd-resolved.service` (DNS)
* `polkit.service` (permissions)
* `udisks2.service` (disk mounting)
* `upower.service` (power mgmt)
* `wpa_supplicant.service` (wifi)

---

# ⚡ Big Memory Services (Optional to stop if not needed)

### If you don’t need Apache right now:

```bash
sudo systemctl stop apache2
sudo systemctl disable apache2
```

### If you don’t need MySQL:

```bash
sudo systemctl stop mysql
sudo systemctl disable mysql
```

### If you don’t need Ollama (very heavy RAM usage):

```bash
sudo systemctl stop ollama
sudo systemctl disable ollama
```

(Ollama is usually the biggest memory eater)

---

# ✅ Recommended memory freeing set (best safe combo)

If you want safe cleanup:

```bash
sudo systemctl stop cups cups-browsed bluetooth avahi-daemon ModemManager whoopsie kerneloops fwupd anydesk
sudo systemctl disable cups cups-browsed bluetooth avahi-daemon ModemManager whoopsie kerneloops fwupd anydesk
```

---

# 🔥 Free memory immediately after stopping services

```bash
sudo sync; echo 3 | sudo tee /proc/sys/vm/drop_caches
```

And check memory:

```bash
free -h
```

---

# =====================================================

# ✅ AUDIT LOGGING (who did what + timestamp)

# =====================================================

For **audit logs** in Linux best tool is:

✅ `auditd`

It logs:

* file changes
* login attempts
* sudo usage
* command execution
* config changes
* timestamp included

---

## Step 1: Install auditd

```bash
sudo apt update
sudo apt install auditd audispd-plugins -y
```

Enable and start:

```bash
sudo systemctl enable auditd
sudo systemctl start auditd
```

Check status:

```bash
systemctl status auditd
```

---

# ✅ Step 2: Track all sudo commands (important)

Audit who executed sudo:

```bash
sudo auditctl -w /etc/sudoers -p wa -k sudoers-change
sudo auditctl -w /etc/sudoers.d/ -p wa -k sudoers-change
```

---

# ✅ Step 3: Track service stop/start/restart changes

(systemctl usage)

```bash
sudo auditctl -w /bin/systemctl -p x -k systemctl-actions
sudo auditctl -w /usr/bin/systemctl -p x -k systemctl-actions
```

---

# ✅ Step 4: Track SSH logins

```bash
sudo auditctl -w /etc/ssh/sshd_config -p wa -k ssh-config-change
sudo auditctl -w /var/log/auth.log -p wa -k ssh-logins
```

---

# ✅ Step 5: Track Apache/MySQL config changes

```bash
sudo auditctl -w /etc/apache2/ -p wa -k apache-change
sudo auditctl -w /etc/mysql/ -p wa -k mysql-change
```

---

# ✅ Step 6: Track important system config changes

```bash
sudo auditctl -w /etc/passwd -p wa -k user-change
sudo auditctl -w /etc/shadow -p wa -k user-change
sudo auditctl -w /etc/group  -p wa -k user-change
```

---

# =====================================================

# 📌 Step 7: Make audit rules permanent (IMPORTANT)

# =====================================================

Temporary rules reset after reboot.

Edit:

```bash
sudo nano /etc/audit/rules.d/audit.rules
```

Paste this full block:

```bash
-w /etc/passwd -p wa -k user-change
-w /etc/shadow -p wa -k user-change
-w /etc/group -p wa -k user-change

-w /etc/sudoers -p wa -k sudoers-change
-w /etc/sudoers.d/ -p wa -k sudoers-change

-w /etc/ssh/sshd_config -p wa -k ssh-config-change

-w /etc/apache2/ -p wa -k apache-change
-w /etc/mysql/ -p wa -k mysql-change

-w /bin/systemctl -p x -k systemctl-actions
-w /usr/bin/systemctl -p x -k systemctl-actions
```

Restart auditd:

```bash
sudo systemctl restart auditd
```

---

# =====================================================

# 🔍 How to view audit logs with time entry

# =====================================================

Audit logs stored in:

📌 `/var/log/audit/audit.log`

### Search systemctl usage:

```bash
sudo ausearch -k systemctl-actions
```

### Search sudo changes:

```bash
sudo ausearch -k sudoers-change
```

### Search SSH config edits:

```bash
sudo ausearch -k ssh-config-change
```

---

# ✅ Convert audit logs into readable report (with time)

```bash
sudo aureport -x --summary
```

Or detailed:

```bash
sudo aureport -x
```

---

# =====================================================

# ⭐ Best Extra Logging (recommended)

# =====================================================

## Enable command history logging for all users

Edit:

```bash
sudo nano /etc/profile
```

Add at bottom:

```bash
export HISTTIMEFORMAT="%F %T "
export PROMPT_COMMAND='history -a'
```

Now every command history will have timestamp.

---

# =====================================================

# 🔐 Recommended Security Logging (best)

# =====================================================

Install rsyslog + logrotate already running (good).

To monitor SSH logins:

```bash
sudo tail -f /var/log/auth.log
```

---

# =====================================================

# FINAL RECOMMENDATION (Best Setup)

# =====================================================

### Stop safe services:

✅ cups, cups-browsed, bluetooth, avahi, modemmanager, fwupd, whoopsie, kerneloops, anydesk

### Stop heavy services if not needed:

⚠️ apache2, mysql, ollama

### Enable audit:

✅ auditd + permanent rules

---