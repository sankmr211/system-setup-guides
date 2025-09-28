Awesome 🚀 glad it’s working!
Here’s a **complete documentation in Markdown (MD) format** that you can keep as a reference. It covers **Ubuntu server setup**, **systemd startup service**, and **remote wakeup from Windows**.

---

# Wake-on-LAN (WOL) Setup and Remote Wake Documentation

This document explains how to enable **Wake-on-LAN (WOL)** on an Ubuntu Server, make it persistent across reboots, and wake it up remotely from a Windows PC.

---

## 📌 1. Enable Wake-on-LAN in BIOS/UEFI
1. Reboot the server.
2. Enter BIOS/UEFI settings.
3. Locate **Power Management / Advanced Settings**.
4. Enable one of the following (name varies by vendor):
   - **Wake on LAN**
   - **PCI-E Wake Up**
   - **PME Event Wake**
5. Save changes and reboot.

---

## 📌 2. Enable WOL on Ubuntu Server

### 2.1 Check Interface
Run:
```bash
ip link show


Example output:

2: enp2s0f1: <BROADCAST,MULTICAST,UP,LOWER_UP> mtu 1500 qdisc fq_codel state UP mode DEFAULT group default qlen 1000
    link/ether 00:1a:2b:3c:4d:5e brd ff:ff:ff:ff:ff:ff
```

* Interface name: `enp2s0f1`
* MAC address: `00:1a:2b:3c:4d:5e`

---

### 2.2 Verify WOL Support

```bash
sudo ethtool enp2s0f1
```

Look for:

```
Supports Wake-on: pumbg
Wake-on: d
```

* `g` = Magic Packet supported ✅
* `d` = Disabled ❌

---

### 2.3 Enable WOL (temporary)

```bash
sudo ethtool -s enp2s0f1 wol g
```

Verify:

```bash
sudo ethtool enp2s0f1 | grep "Wake-on"
```

Expected:

```
Wake-on: g
```

---

## 📌 3. Make WOL Persistent (Ubuntu Startup)

### 3.1 Create a systemd Service

Create the file:

```bash
sudo nano /etc/systemd/system/wol.service
```

Paste:

```ini
[Unit]
Description=Enable Wake-on-LAN for enp2s0f1
After=network-online.target
Wants=network-online.target

[Service]
Type=oneshot
ExecStart=/usr/sbin/ethtool -s enp2s0f1 wol g

[Install]
WantedBy=multi-user.target
```

---

### 3.2 Enable and Start Service

```bash
sudo systemctl daemon-reexec
sudo systemctl enable wol.service
sudo systemctl start wol.service
```

---

### 3.3 Verify After Reboot

```bash
sudo reboot
sudo ethtool enp2s0f1 | grep "Wake-on"
```

Expected:

```
Wake-on: g
```

---

## 📌 4. Remote Wake

### 4.1 Install WakeMeOnLan Windows (Recommended)

1. Download: [WakeMeOnLan](https://www.nirsoft.net/utils/wake_on_lan.html)
2. Run `WakeMeOnLan.exe`.
3. Go to **File → Add New Computer** and enter:

   * **MAC Address**: `00:1a:2b:3c:4d:5e`
   * **IP Address**: Ubuntu server’s local IP (e.g., `192.168.1.50`)
4. Right-click the entry → **Wake Up Selected Computers**.

---

### 4.1.1 PowerShell Alternative

Open **PowerShell as Administrator** and run:

#### Install Module (first time only)

```powershell
Install-Module -Name WakeOnLan -Force
```

#### Send Wake Packet

```powershell
Send-WolPacket -MacAddress "00:1a:2b:3c:4d:5e" -Broadcast 192.168.1.255
```

Replace:

* `00:1a:2b:3c:4d:5e` → your Ubuntu MAC address
* `192.168.1.255` → your network broadcast IP

---

### 4.2 Install WakeMeOnLan Linux

Install the tool:

```bash
sudo apt install wakeonlan
```

Send wake packet:

```bash
wakeonlan 00:1a:2b:3c:4d:5e
```

---

### 4.3 Install WakeMeOnLan Mobile Apps

Use a **Wake-on-LAN app** on Android or iOS:

* **Android**: "Wake On LAN", "WOL" (available on Play Store)
* **iOS**: "Mocha WOL", "Wake On Lan"
* Enter:

  * **MAC Address**: `00:1a:2b:3c:4d:5e`
  * **IP Address**: Ubuntu server’s LAN IP (e.g., `192.168.1.50`)
  * **Broadcast**: `192.168.1.255`

---

## 📌 5. Testing the Setup

1. **Shutdown Ubuntu**:

   ```bash
   sudo shutdown now
   ```
2. From **Windows PC**, send a WOL packet using WakeMeOnLan or PowerShell.
3. The Ubuntu server should **power on automatically** 🎉

---



