Here's how to install **n8n** (a powerful open-source workflow automation tool) on **Ubuntu**:

---

#  1. Installation Workflow

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








# 2. Remove Workflow


### 🔹 1. Remove Workflow via n8n Editor UI

1. Run n8n:

   ```bash
   n8n
   ```

   (or `n8n start` if you set it up as a service).
2. Open **[http://localhost:5678](http://localhost:5678)** in your browser.
3. Go to the **Workflows** tab (left sidebar).
4. Find the workflow → click **⋮ (three dots)** → **Delete**.
5. Confirm deletion.

---

### 🔹 2. Remove Workflow via Command Line (if UI not working)

Since you installed with npm, workflows are stored in an SQLite DB in your home folder:

* Location:

  ```
  ~/.n8n/database.sqlite
  ```

To delete a workflow manually:

1. Install SQLite CLI (if not installed):

   ```bash
   sudo apt-get install sqlite3 -y   # Ubuntu/Debian
   ```
2. Open the database:

   ```bash
   sqlite3 ~/.n8n/database.sqlite
   ```
3. List all workflows:

   ```sql
   SELECT id, name FROM workflow_entity;
   ```
4. Delete a workflow by ID:

   ```sql
   DELETE FROM workflow_entity WHERE id = 3;
   ```
5. Exit SQLite:

   ```sql
   .quit
   ```

Restart n8n:

```bash
n8n
```

---

## 🧠 Tips

| Feature                 | How                                             |
| ----------------------- | ----------------------------------------------- |
| Access n8n from browser | [http://localhost:5678](http://localhost:5678)  |
| Make it public          | Use NGINX reverse proxy or change `N8N_HOST`    |
| Set custom port         | `export N8N_PORT=1234` before `n8n`             |
| Use env file            | Use `.env` and `n8n start` if installed locally |

---







# 3. HTTPS Run

Yes 👍 you’ll need to **generate SSL certificates** (`privkey.pem` and `fullchain.pem`) before n8n can run with HTTPS.
There are two main ways:

---

## 🔹 1. **For Production (Free SSL from Let’s Encrypt)**

If you have a **domain name** pointing to your server (e.g., `n8n.example.com`):

1. Install **Certbot** (Let’s Encrypt client):

   ```bash
   sudo apt update
   sudo apt install certbot
   ```

2. Run Certbot to get certificates:

   ```bash
   sudo certbot certonly --standalone -d n8n.example.com
   ```

   This will create cert files in:

   ```
   /etc/letsencrypt/live/n8n.example.com/privkey.pem
   /etc/letsencrypt/live/n8n.example.com/fullchain.pem
   ```

3. Point n8n env variables to them:

   ```bash
   export N8N_PROTOCOL=https
   export N8N_SSL_KEY=/etc/letsencrypt/live/n8n.example.com/privkey.pem
   export N8N_SSL_CERT=/etc/letsencrypt/live/n8n.example.com/fullchain.pem
   n8n start
   ```

   👉 Now you can open `https://n8n.example.com`

⚡ These certs auto-renew with `certbot renew`.

---

## 🔹 2. **For Local / Testing (Self-Signed Certificate)**

If you don’t have a domain:

1. Generate a self-signed cert:

   ```bash
   mkdir ~/certs
   openssl req -x509 -nodes -days 365 \
     -newkey rsa:2048 \
     -keyout ~/certs/privkey.pem \
     -out ~/certs/fullchain.pem
   ```

   (Just press Enter or fill info when asked.)

2. Run n8n with:

   ```bash
   export N8N_PORT=8443
   export N8N_PROTOCOL=https
   export N8N_SSL_KEY=~/certs/privkey.pem
   export N8N_SSL_CERT=~/certs/fullchain.pem
   export WEBHOOK_URL=https://localhost:5678 (domain)
   n8n start
   ```

3. Open in browser: `https://localhost:5678`
   (Browser will show *“Not Secure”* warning because it’s self-signed — safe for testing only.)

---

# 4. Public Access & Integrations

For external webhook integrations (e.g., **Telegram**, **Discord**, or other APIs**), you can expose your local n8n server securely.

### 📡 Expose n8n Publicly via ngrok  
➡️ [See full guide → `ngrok.md`](./ngrok.md)

### 🤖 Connect Telegram Bot with n8n  
➡️ [See full guide → `telegram.md`](./telegram.md)

