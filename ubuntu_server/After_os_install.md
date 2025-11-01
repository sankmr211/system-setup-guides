# 🧠 **Ubuntu Server — Post Installation Setup**

After installing **Ubuntu Server**, you’ll have a **minimal base system**.
To make it ready for daily use, management, networking, monitoring, and security — install these essential tools.

---

## 🧩 **1️⃣ System Basics**

Essential utilities for command-line use and management:

```bash
sudo apt install -y \
  vim \
  nano \
  curl \
  wget \
  git \
  unzip \
  zip \
  tar \
  htop \
  screen \
  tmux \
  lsb-release \
  ca-certificates \
  software-properties-common \
  net-tools
```

**What they do:**

| Tool                             | Description                                        |
| -------------------------------- | -------------------------------------------------- |
| `vim`, `nano`                    | Text editors                                       |
| `curl`, `wget`                   | Download files & test network                      |
| `git`                            | Version control                                    |
| `unzip`, `zip`, `tar`            | Archive tools                                      |
| `htop`                           | Process monitor                                    |
| `screen`, `tmux`                 | Terminal multiplexer (persistent sessions via SSH) |
| `lsb-release`, `ca-certificates` | System info & SSL                                  |
| `net-tools`                      | Legacy tools (`ifconfig`, `netstat`, etc.)         |

---

## 🧩 **2️⃣ Network & SSH Tools**

Network utilities and SSH access setup:

```bash
sudo apt install -y \
  openssh-server \
  nmap \
  traceroute \
  iputils-ping \
  dnsutils
```

**What they do:**

| Tool             | Description                      |
| ---------------- | -------------------------------- |
| `openssh-server` | Enables SSH login to your server |
| `nmap`           | Network/port scanner             |
| `traceroute`     | Shows network route hops         |
| `iputils-ping`   | Ping utility                     |
| `dnsutils`       | For `dig`, `nslookup`            |

---

## 🧩 **3️⃣ Storage & Disk Management**

Tools for partitions, volumes, and backups:

```bash
sudo apt install -y \
  lvm2 \
  gdisk \
  parted \
  rsync \
  ncdu
```

**What they do:**

| Tool              | Description                                  |
| ----------------- | -------------------------------------------- |
| `lvm2`            | Logical Volume Manager (resize/manage disks) |
| `gdisk`, `parted` | Disk partitioning tools                      |
| `rsync`           | File sync & backups                          |
| `ncdu`            | Visual disk usage analyzer in terminal       |

---

## 🧩 **4️⃣ System Monitoring & Logs**

Performance and log management tools:

```bash
sudo apt install -y \
  sysstat \
  iotop \
  dstat \
  vnstat \
  bmon \
  logrotate
```

**What they do:**

| Tool             | Description                                  |
| ---------------- | -------------------------------------------- |
| `sysstat`        | CPU/memory stats (via `sar`)                 |
| `iotop`          | Disk I/O monitor                             |
| `dstat`          | Performance overview                         |
| `vnstat`, `bmon` | Network bandwidth monitors                   |
| `logrotate`      | Rotates & compresses log files automatically |

---

## 🧩 **5️⃣ Security & Firewall**

Basic protection and automatic patching:

```bash
sudo apt install -y \
  ufw \
  fail2ban \
  unattended-upgrades
```

**What they do:**

| Tool                  | Description                    |
| --------------------- | ------------------------------ |
| `ufw`                 | Uncomplicated Firewall         |
| `fail2ban`            | Blocks brute-force SSH attacks |
| `unattended-upgrades` | Automatic security updates     |

**Enable firewall safely:**

```bash
sudo ufw allow OpenSSH
sudo ufw enable
sudo ufw status
```

---

## 🧩 **6️⃣ Backup & Transfer Tools**

For remote syncing and cloud backups:

```bash
sudo apt install -y \
  rsync \
  rclone \
  sshfs
```

**What they do:**

| Tool     | Description                                       |
| -------- | ------------------------------------------------- |
| `rsync`  | Efficient local & remote synchronization          |
| `rclone` | Syncs with cloud storage (Google Drive, S3, etc.) |
| `sshfs`  | Mount remote directories over SSH                 |

---

## 🧩 **7️⃣ Diagnostic & Recovery Tools**

Network and system troubleshooting tools:

```bash
sudo apt install -y \
  netcat \
  lsof \
  psmisc \
  tcpdump \
  strace
```

**What they do:**

| Tool      | Description                           |
| --------- | ------------------------------------- |
| `netcat`  | Open/test network ports               |
| `lsof`    | List open files and ports             |
| `psmisc`  | Utilities like `killall` and `pstree` |
| `tcpdump` | Capture network packets               |
| `strace`  | Trace system calls and processes      |

---

## 🧩 **8️⃣ Convenience Tools (Optional)**

For better usability and quick system insights:

```bash
sudo apt install -y \
  neofetch \
  figlet \
  tree \
  mc
```

**What they do:**

| Tool       | Description                                |
| ---------- | ------------------------------------------ |
| `neofetch` | Display system info summary                |
| `figlet`   | Generate ASCII art banners                 |
| `tree`     | Show directory tree structure              |
| `mc`       | Midnight Commander — terminal file manager |

---

## 🧩 **9️⃣ Verify Ubuntu Version**

```bash
lsb_release -a
```

**Example output:**

```
Description: Ubuntu 22.04.5 LTS
Codename:    jammy
```

---

## 🧩 **🔐 Enable SSH (if not already running)**

```bash
sudo systemctl status ssh
```

If SSH is **not installed**, do:

```bash
sudo apt update
sudo apt install openssh-server -y
sudo systemctl enable ssh
sudo systemctl start ssh
sudo systemctl status ssh
```

---

## 🧩 **🔧 Network Setup Summary**

**Bring up interface manually:**

```bash
ip link show
sudo ip link set <interface_name> up
ip a
```

**Netplan (for Ethernet):**

```bash
sudo nano /etc/netplan/01-netcfg.yaml
```

copy and paste below the code

```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    <interface_name>:
      dhcp4: true
```

**Apply changes:**

```bash
sudo netplan apply
networkctl
```

**View status:**

```bash
networkctl status <interface_name>
```

**Show IPs only:**

```bash
networkctl list --no-pager
# or
networkctl status <interface_name> | grep "Address"
```

**Restart network service:**

```bash
sudo systemctl restart systemd-networkd
```

---

## 🧩 **🔗 Network Manager (GUI / nmcli)**

```bash
sudo apt install network-manager -y
sudo ip link set <interface_name> up
```

**Check devices:**

```bash
nmcli device status
```

**Connect to Wi-Fi:**

```bash
nmcli device wifi list
sudo nmcli device wifi connect "<SSID>" password "<password>" ifname <wifi_interface_name>
```

**Saved Wi-Fi:**

```bash
sudo nmcli connection <up | down>  "<SSID>"
```

**Check connection:**

```bash
nmcli device status
nmcli connection show --active
ip a show <wifi_interface_name>
```

**Reconnect / Disconnect:**

```bash
nmcli device disconnect <wifi_interface_name>
nmcli device connect <wifi_interface_name>
```


**Autoconnect:**

```bash
nmcli -f NAME,AUTOCONNECT connection show
nmcli connection show "<SSID>" | grep autoconnect
sudo nmcli connection modify "<SSID>" connection.autoconnect <yes | no>
```

---
