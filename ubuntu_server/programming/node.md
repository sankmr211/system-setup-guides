### 🔧 1. **Install Node.js and npm**#### 📥 Use NodeSource (for latest LTS):

```bash
curl -fsSL https://deb.nodesource.com/setup_18.x | sudo -E bash -
sudo apt install -y nodejs
```

Check version:

```bash
node -v
>v18.20.8
npm -v
>10.8.2
```

### 🔧 2. **Install NMV**:

Initial Steps:
```bash
sudo apt update
curl -o- https://raw.githubusercontent.com/nvm-sh/nvm/v0.40.0/install.sh | bash
source ~/.bashrc
```

Check version:
```bash
nvm --version
>0.40.0
```


List installed versions:
```bash
nvm ls
```

List versions:
```bash
nvm ls-remote
```

Install version:
```bash
nvm install <verson>
```

Uninstall version:
```bash
nvm uninstall <verson>
```

Switch versions:
```bash
nvm use <version>
```

Set Default version:
```bash
nvm alias default <version>
```
---