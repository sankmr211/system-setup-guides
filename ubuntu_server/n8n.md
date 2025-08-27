Here's how to install **n8n** (a powerful open-source workflow automation tool) on **Ubuntu**:

---

## ✅ Option 1: Install via Docker (Recommended)

### 🔧 Prerequisites:

* Ubuntu 20.04 or later
* Docker installed
* Docker Compose installed

### 🪛 Step-by-Step:

1. **Install Docker** (if not already):

```bash
sudo apt update
sudo apt install -y docker.io
sudo systemctl enable docker
sudo systemctl start docker
```

2. **Install Docker Compose** (if not already):

```bash
sudo apt install -y docker-compose
```

3. **Create a project folder**:

```bash
mkdir n8n-docker && cd n8n-docker
```

4. **Create `docker-compose.yml`**:

```yaml
version: '3'

services:
  n8n:
    image: n8nio/n8n
    restart: always
    ports:
      - "5678:5678"
    environment:
      - N8N_BASIC_AUTH_ACTIVE=true
      - N8N_BASIC_AUTH_USER=admin
      - N8N_BASIC_AUTH_PASSWORD=admin123
    volumes:
      - ~/.n8n:/home/node/.n8n
```

5. **Start n8n**:

```bash
docker-compose up -d
```

6. **Open your browser**:

Go to: [http://localhost:5678](http://localhost:5678)

> Log in with:
>
> * **User**: `admin`
> * **Password**: `admin123`

---

## ✅ Option 2: Install Without Docker (Node.js)

---

### 🔧 1. **Install Node.js and npm**

n8n requires Node.js **v18+** (LTS version recommended).

#### 📥 Use NodeSource (for latest LTS):

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Check version:

```bash
node -v
npm -v
```

---

### 🔧 2. **Install n8n globally**

```bash
sudo npm install -g n8n
```

---

### ✅ 3. **Start n8n**

Just run:

```bash
n8n
```

> This will start a local server on [http://localhost:5678](http://localhost:5678)

---

### ⚙️ 4. (Optional) Enable Basic Auth (Username/Password)

Before starting `n8n`, export the following environment variables:

```bash
export N8N_BASIC_AUTH_ACTIVE=true
export N8N_BASIC_AUTH_USER=admin
export N8N_BASIC_AUTH_PASSWORD=admin123
n8n
```

---

### 🚀 5. Run n8n in Background Using PM2 (Recommended)

To run `n8n` in the background and start it on boot:

```bash
sudo npm install -g pm2
pm2 start n8n
pm2 save
pm2 startup
```

Now n8n will auto-run even after reboot.

---

## 📁 Data Location (Non-Docker)

n8n will save data (workflows, credentials) in:

```bash
~/.n8n
```

You can backup/export this directory anytime.

---

## 🧠 Tips

| Feature                 | How                                             |
| ----------------------- | ----------------------------------------------- |
| Access n8n from browser | [http://localhost:5678](http://localhost:5678)  |
| Make it public          | Use NGINX reverse proxy or change `N8N_HOST`    |
| Set custom port         | `export N8N_PORT=1234` before `n8n`             |
| Use env file            | Use `.env` and `n8n start` if installed locally |

---


