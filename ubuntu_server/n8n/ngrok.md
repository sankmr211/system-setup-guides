# 🌍 Expose N8N via ngrok

Using **ngrok** is the fastest and easiest way to make your local n8n instance accessible on the internet — perfect for testing webhooks or Telegram integrations.

---

## 🔹 1. Install ngrok

```bash
# Download ngrok (Linux)
wget https://bin.equinox.io/c/bNyj1mQVY4c/ngrok-stable-linux-amd64.zip
unzip ngrok-stable-linux-amd64.zip
sudo mv ngrok /usr/local/bin/

# Authenticate with your ngrok account
ngrok config add-authtoken <YOUR_NGROK_AUTHTOKEN>
````

---

## 🔹 2. Run n8n with HTTPS (port 8443)

Make sure your n8n server runs securely:

```bash
export N8N_PORT=8443
export N8N_PROTOCOL=https
export N8N_SSL_KEY=/certs/privkey.pem
export N8N_SSL_CERT=/certs/fullchain.pem
n8n start
```

---

## 🔹 3. Start ngrok Tunnel

```bash
ngrok http 8443
```

You’ll get a public forwarding URL like:

```
Forwarding    https://abcd1234.ngrok.io -> https://localhost:8443
```

---

## 🔹 4. Update n8n Webhook URL

Tell n8n to use your ngrok HTTPS URL:

```bash
export WEBHOOK_URL=https://abcd1234.ngrok.io/
n8n start
```

> 💡 If you’re using Docker:
>
> ```bash
> docker run -e WEBHOOK_URL=https://abcd1234.ngrok.io/ n8nio/n8n
> ```

---