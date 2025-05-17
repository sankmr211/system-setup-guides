# ⚙️ Service Setup Scripts

This repository contains **installation, configuration, and uninstallation scripts** for commonly used server-side services on **Ubuntu/Linux systems**. Each service is organized into its own directory with a dedicated script and documentation.

---

## 📁 Services Included

### 🐰 [RabbitMQ](./rabbitmq/)

- Version: 4.1.0
- Install via Cloudsmith APT mirror
- Includes RabbitMQ Management Plugin setup
- Full uninstallation instructions

➡️ See: [`rabbitmq/README.md`](./rabbitmq/README.md)

---

### 🧠 [Redis](./redis/)

- Install via APT and Snap (optional)
- Configuration via official Redis.io mirror
- Clean uninstall and process cleanup

➡️ See: [`redis/README.md`](./redis/README.md)

---

### 🐘 [PostgreSQL](./postgres/)

- Install PostgreSQL with common extensions
- Configure system service and users
- Full uninstall cleanup steps

➡️ See: [`postgres/README.md`](./postgres/README.md)

---

## 🧰 Usage

Each directory contains:

- A `setup.sh` or `service.sh` script (e.g., `rabbitmq.sh`)
- A `README.md` file with:
  - Step-by-step install instructions
  - Service management tips
  - Uninstallation and cleanup

You can run a script directly like this:

```bash
cd rabbitmq
chmod +x rabbitmq.sh
./rabbitmq.sh
