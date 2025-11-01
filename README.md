# ⚙️ Service Setup Scripts

A curated collection of **installation, configuration, and uninstallation scripts** for essential server and desktop services on **Ubuntu/Linux systems**.  
Each service includes a dedicated setup script, detailed documentation, and cleanup instructions — making deployment fast, reliable, and repeatable.

---

## 📚 Table of Contents

- [Overview](#-overview)
- [Included Services](#-included-services)
  - [🐰 RabbitMQ](#-rabbitmq)
  - [🧠 Redis](#-redis)
- [Ubuntu Desktop Tools](#-ubuntu-desktop-tools)
  - [💻 Hardinfo](#-hardinfo)
  - [📊 RAM Monitor](#-ram-monitor)
- [Ubuntu Server Utilities](#-ubuntu-server-utilities)
  - [🔌 Server Port Change](#-server-port-change)
  - [🧩 N8N Workflow](#-n8n-workflow)
  - [📦 NPM Global Setup](#-npm-global-setup)
  - [💾 Memory Commands](#-memory-commands)
  - [🗂️ File System Tools](#-file-system-tools)
  - [🌅 Server Wakeup Automation](#-server-wakeup-automation)
  - [🚀 Post-Install Essentials](#-post-install-essentials)
  - [🚀 Install Nodejs & NVM](#-Install-Nodejs-&-NVM)
  - [🚀 Install Python Pyenv](#-Install-Python-Pyenv)
- [License](#-license)

---

## 🌍 Overview

This repository is designed to streamline **DevOps and system administration tasks** through a collection of well-documented Bash scripts.  
Each service directory includes:

- 🔧 **Install Script** – Fully automated setup with dependencies  
- 🧹 **Uninstall Script** – Clean removal of packages and configs  
- 📘 **README.md** – Explains usage, configuration, and troubleshooting  

---

## 🐰 RabbitMQ

> **Version:** 4.1.0  
> **Source:** Cloudsmith APT mirror  

**Features:**
- Automated installation of RabbitMQ and Erlang dependencies  
- Enables **RabbitMQ Management Plugin** for web-based monitoring  
- Includes **secure configuration and uninstallation** instructions  

📄 Documentation: [`rabbitmq/README.md`](./rabbitmq/README.md)

---

## 🧠 Redis

> **Installation Options:** APT or Snap  
> **Source:** Official Redis.io APT mirror  

**Features:**
- Quick Redis installation and service configuration  
- Optional Snap-based setup  
- Includes **uninstall and process cleanup** script  

📄 Documentation: [`redis/README.md`](./redis/README.md)

---

## 💻 Ubuntu Desktop Tools

### 🧮 Hardinfo

**Features:**
- Generates comprehensive **hardware and benchmark reports**  
- Exports data in **HTML format** for easy sharing  
- Useful for **system audits and performance comparisons**

📄 Documentation: [`ubuntu_desktop/README.md`](./ubuntu_desktop/README.md)

---

### 📊 RAM Monitor

**Features:**
- Real-time system memory usage tracking  
- Helps diagnose **memory-intensive processes**  
- Lightweight monitoring utility for desktops  

📄 Documentation: [`ubuntu_desktop/ram_monti.md`](./ubuntu_desktop/ram_monti.md)

---

## 🧱 Ubuntu Server Utilities

### 🔌 Server Port Change
Modify default port configurations safely and persistently.  
📄 [`ubuntu_server/server_port.md`](./ubuntu_server/server_port.md)

---

### 🧩 N8N Workflow
Automated setup for **N8N**, the open-source workflow automation tool.  
📄 [`ubuntu_server/n8n/n8n.md`](./ubuntu_server/n8n/n8n.md)

---

### 📦 NPM Global Setup
Configures Node.js and global NPM packages for CI/CD or deployment.  
📄 [`ubuntu_server/npm.md`](./ubuntu_server/npm.md)

---

### 💾 Memory Commands
Useful memory and performance-related command shortcuts for servers.  
📄 [`ubuntu_server/memory.md`](./ubuntu_server/memory.md)

---

### 🗂️ File System Tools
Scripts for checking, cleaning, and maintaining file systems efficiently.  
📄 [`ubuntu_server/file_system.md`](./ubuntu_server/file_system.md)

---

### 🌅 Server Wakeup Automation
Automates system startup and scheduled wake-on-LAN setups.  
📄 [`ubuntu_server/server_wakeup.md`](./ubuntu_server/server_wakeup.md)

---

### 🚀 Post-Install Essentials
A curated script to install **core software** required after a fresh OS setup.  
📄 [`ubuntu_server/After_os_install.md`](./ubuntu_server/After_os_install.md)

---

### 🚀 Install Nodejs & NVM 
Install Node 18 and install nvm for manage version.  
📄 [`ubuntu_server/programming/node.md`](./ubuntu_server/programming/node.md)

---

### 🚀 Install Python Pyenv 
Install Node 18 and install nvm for manage version.  
📄 [`ubuntu_server/programming/python.md`](./ubuntu_server/programming/python.md)

---

## 🧾 License

This repository is released under the **MIT License** — you’re free to use, modify, and distribute these scripts with attribution.

---

> 💡 **Pro Tip:**  
> Run scripts with `sudo` privileges for smooth installation, and review each `README.md` before execution for configuration details.
