### **monitor RAM usage on Ubuntu** directly from the **notification bar (top bar/system tray)**

---

### ✅ Option 1: **Use GNOME Extension - System Monitor**

**Steps:**

1. Install the GNOME Shell Extensions support:

   ```bash
   sudo apt install gnome-shell-extensions
   ```

2. Install browser integration for extensions:

   * Go to [https://extensions.gnome.org](https://extensions.gnome.org)
   * Install the browser plugin if prompted.

3. Install the **System Monitor** extension:

   * Go to: [https://extensions.gnome.org/extension/120/system-monitor/](https://extensions.gnome.org/extension/120/system-monitor/)
   * Toggle to ON and allow installation.

4. Configure the extension:

   * You can display CPU, RAM, network, etc.
   * It appears in the top panel with live stats.

---

### ✅ Option 2: **Use Indicator Multiload (simple and effective)**

**Steps:**

1. Install the package:

   ```bash
   sudo apt install indicator-multiload
   ```

2. Launch it:

   ```bash
   indicator-multiload &
   ```

3. It will appear in the notification area and show **CPU, memory, network, and disk usage** in small graph icons.

4. To autostart on boot:

   * Go to **Startup Applications**
   * Add: `indicator-multiload`

---

### ✅ Option 3: **Use Conky (advanced customization)**

Conky can display RAM (and more) on your desktop, but **not in the top bar**. It's useful if you want a desktop overlay instead of a tray icon.

Install:

```bash
sudo apt install conky
```

Customize via `~/.conkyrc`.

---

### Summary

| Tool                 | Display Location  | Features                   |
| -------------------- | ----------------- | -------------------------- |
| GNOME System Monitor | Top bar           | CPU, RAM, network, storage |
| Indicator Multiload  | Notification area | Simple graphs              |
| Conky                | Desktop overlay   | Highly customizable        |
