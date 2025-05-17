# 🐰 RabbitMQ 4.1.0 Installation & Uninstallation Guide (Ubuntu)

This repository provides scripts and instructions to **install** or **fully uninstall** RabbitMQ 4.1.0 on **Ubuntu** using the official Cloudsmith APT repository (as per RabbitMQ 3.13+ documentation).

---

## 📁 Contents

- [Prerequisites](#prerequisites)
- [Installation (RabbitMQ 4.1.0)](#installation-rabbitmq-410)
  - [Install using Quick Start Script](#install-using-quick-start-script)
  - [Enable RabbitMQ Management Plugin](#enable-rabbitmq-management-plugin)
- [Uninstallation (Complete Cleanup)](#uninstallation-complete-cleanup)
- [Verification](#verification)
- [References](#references)

---

## ✅ Prerequisites

- Ubuntu 20.04, 22.04, or newer
- `sudo` privileges
- Internet connection

---

## 🚀 Installation (RabbitMQ 4.1.0)

### 📜 Install using Quick Start Script

> The installation script is based on [RabbitMQ's official guide](https://www.rabbitmq.com/docs/3.13/install-debian#apt-cloudsmith).

1. Download and execute the script:

```bash
chmod +x rabbitmq.sh
./rabbitmq.sh

2. Verify RabbitMQ is running:

```bash
systemctl status rabbitmq-server.service
```

---

### 🔌 Enable RabbitMQ Management Plugin

1. List all available plugins:

```bash
sudo rabbitmq-plugins list
```

2. Enable the web-based management interface:

```bash
sudo rabbitmq-plugins enable rabbitmq_management
```

3. Access the management UI:

* URL: [http://localhost:15672](http://localhost:15672)
* Default credentials:

  * Username: `guest`
  * Password: `guest`

---

## ❌ Uninstallation (Complete Cleanup)

To completely remove RabbitMQ and its dependencies:

### 1. Stop the RabbitMQ service

```bash
sudo systemctl stop rabbitmq-server
```

### 2. Uninstall RabbitMQ and Erlang

```bash
sudo apt-get purge rabbitmq-server erlang* -y
```

### 3. Remove residual files and directories

```bash
sudo rm -rf /var/lib/rabbitmq/
sudo rm -rf /etc/rabbitmq/
sudo rm -rf /usr/lib/rabbitmq/
sudo rm -rf /var/log/rabbitmq/
sudo rm -rf /etc/default/rabbitmq-server
sudo rm -rf /etc/init.d/rabbitmq-server
```

### 4. Remove RabbitMQ user (if created manually)

```bash
sudo deluser rabbitmq
```

### 5. Clean up unnecessary packages

```bash
sudo apt-get autoremove -y
```

---

## 🔍 Verification

Check that RabbitMQ has been removed successfully:

```bash
systemctl status rabbitmq-server
```

Expected output:

```
Unit rabbitmq-server.service could not be found.
```

---

## 📚 References

* [RabbitMQ Official Installation Guide (3.13+)](https://www.rabbitmq.com/docs/3.13/install-debian)
* [RabbitMQ GitHub](https://github.com/rabbitmq)
* [Erlang Solutions](https://www.erlang-solutions.com/resources/download.html)

---

## 📝 License

This project is open-source and available under the [MIT License](LICENSE).

```

---

### ✅ Optional: Include `rabbitmq.sh`

Let me know if you want me to generate the `rabbitmq.sh` script referenced in this README so it’s a fully working repository.
```