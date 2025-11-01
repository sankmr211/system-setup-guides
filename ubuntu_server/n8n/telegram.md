# 🤖 Connect Telegram Bot with N8N

Use **Telegram Trigger nodes** to build real-time workflows such as chat automation, notifications, and command-based actions.

---

## 🔹 1. Create a Telegram Bot

1. Open Telegram and search for **@BotFather**.  
2. Use `/newbot` to create a new bot.  
3. Follow the prompts and copy your **Bot Token** (e.g., `123456789:ABCxyz...`).

---

## 🔹 2. Set up n8n Webhook URL

If you’re using **ngrok**, export your ngrok URL to n8n:

```bash
export WEBHOOK_URL=https://abcd1234.ngrok.io/
n8n start
````

Now n8n will register Telegram’s webhook using this public URL.

---

## 🔹 3. Add Telegram Trigger in n8n

1. Open [http://localhost:5678](http://localhost:5678) (or your ngrok URL).
2. Create a **new workflow**.
3. Add the **Telegram Trigger** node.
4. Paste your **Bot Token** from BotFather.
5. Set event type (e.g., "Message Received").
6. Save and activate the workflow.

---

## 🔹 4. Test

Send a message to your Telegram bot.
If configured correctly, your n8n workflow will trigger instantly 🎉

---
