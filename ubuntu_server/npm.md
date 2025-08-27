To **see globally installed npm packages** on your system, use the following commands:

---

## ✅ 1. **List Global Packages**

```bash
npm list -g --depth=0
```

* `-g` — Shows globally installed packages
* `--depth=0` — Only shows top-level packages (not dependencies)

### 📦 Output Example:

```bash
/usr/lib/node_modules
├── n8n@1.42.1
├── pm2@5.3.0
└── npm@10.5.0
```

---

## ✅ 2. **Show Where Global Packages Are Installed**

```bash
npm root -g
```

🔁 Output:

```bash
/usr/lib/node_modules
```

---

## ✅ 3. **Show Global `npm` Binary Location**

```bash
npm bin -g
```

🔁 Output:

```bash
/usr/bin
```

This tells you where global CLI tools (like `n8n`, `pm2`, `eslint`, etc.) are available in your shell.

---

## ✅ 4. **Check Specific Package (e.g., `n8n`)**

```bash
npm list -g n8n
```

Or to see its version:

```bash
npm ls -g n8n --depth=0
```