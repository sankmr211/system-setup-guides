# filesystem works in Ubuntu (Linux)

## 🗂️ 1. The Linux Filesystem Basics

Ubuntu follows the **Linux Filesystem Hierarchy Standard (FHS)**. Unlike Windows (which has `C:\`, `D:\`), Linux has a **single root directory** `/`. Everything starts from `/` and branches out.

```
/
├── bin/       → Essential binaries (commands like ls, cp, mv)
├── boot/      → Boot loader files (GRUB, kernel images)
├── dev/       → Device files (hard disks, USBs, etc.)
├── etc/       → Configuration files (system-wide settings)
├── home/      → User home directories (/home/santhosh, etc.)
├── lib/       → Shared libraries for binaries
├── media/     → Mounted removable media (USB, CD)
├── mnt/       → Temporary mount points
├── opt/       → Optional software
├── proc/      → Virtual files for processes & kernel info
├── root/      → Root user’s home directory
├── run/       → Runtime process info (like PID files)
├── sbin/      → System binaries (admin commands)
├── srv/       → Data for services (web, ftp)
├── sys/       → Kernel & hardware info (like /proc)
├── tmp/       → Temporary files (auto-cleaned)
├── usr/       → Applications and libraries for users
└── var/       → Logs, cache, mail, spool
```

---

## ⚙️ 2. Key Concepts

### **a) Everything is a file**

* Devices (hard disks, printers), processes, configs → all treated as files.
* Example:

  * `/dev/sda` = your hard drive
  * `/dev/null` = black hole (discard output)

---

### **b) File Types**

Run `ls -l` and look at the first character:

* `-` → regular file
* `d` → directory
* `l` → symbolic link
* `c` → character device (like terminal)
* `b` → block device (like hard disk)
* `p` → named pipe
* `s` → socket

---

### **c) Mounting**

Ubuntu doesn't use drive letters. Instead, storage devices are **mounted** into the tree.

Example:

* Hard drive: `/dev/sda1` → mounted at `/` (root)
* USB drive: `/dev/sdb1` → mounted at `/media/santhosh/USB`

Check mounts:

```bash
lsblk
mount | grep "^/"
df -h
```

---

### **d) Permissions**

Each file/directory has:

* **Owner** (user)
* **Group**
* **Others**
* **Permissions**: read (r), write (w), execute (x)

Example:

```bash
-rw-r--r--  1 user group  1234 Aug 24 10:00 file.txt
```

Meaning:

* `rw-` (owner can read/write)
* `r--` (group can read)
* `r--` (others can read)

Change permissions:

```bash
chmod 755 script.sh
chown santhosh:users file.txt
```

---

### **e) Filesystem Types**

Ubuntu supports many:

* `ext4` → default for Linux
* `xfs`, `btrfs` → advanced filesystems
* `vfat`, `ntfs` → Windows filesystems (for USB drives)
* `tmpfs` → memory-based filesystem

---

## 🔍 3. Useful Commands

```bash
pwd             # current directory
ls -l           # list with details
cd /etc         # change directory
du -sh *        # disk usage in current folder
df -h           # free disk space
file filename   # check file type
stat filename   # file details (size, modified time, inode)
```

---

## 🧠 4. How Ubuntu Uses the Filesystem

* At boot → kernel mounts `/`
* Init system loads services from `/etc` and `/usr`
* Users get `/home/username`
* Logs go to `/var/log`
* Temporary stuff goes to `/tmp` or `/run`

---

Got it 👍 let’s go step by step.

---

## 🔑 `chmod` = **Change Mode (permissions)**

Permissions can be set in **two ways**:

1. **Symbolic** (using `u`, `g`, `o`, `a`)
2. **Numeric (octal)** (using 3-digit numbers like `755`)

---

### 4. **Symbolic Method**

```bash
chmod u+r file.txt   # add read to user
chmod g-w file.txt   # remove write from group
chmod o+x file.txt   # add execute for others
chmod a+r file.txt   # give read permission to everyone (all)
```

* `u` = user (owner)
* `g` = group
* `o` = others
* `a` = all (user+group+others)
* `+` = add
* `-` = remove
* `=` = set exactly

---

### 5. **Numeric (Octal) Method**

Each permission has a number:

* `r = 4`
* `w = 2`
* `x = 1`

Add them together →

* `7 = rwx`
* `6 = rw-`
* `5 = r-x`
* `4 = r--`
* `0 = ---`

👉 Example:

```bash
chmod 755 mydir
```

\= `rwxr-xr-x`

* User = `7` (rwx)
* Group = `5` (r-x)
* Others = `5` (r-x)

---

### 6. **Common Permission Examples**

* **Private file (only you can read/write):**

  ```bash
  chmod 600 file.txt    # -rw-------
  ```

* **Executable script:**

  ```bash
  chmod 755 script.sh   # -rwxr-xr-x
  ```

* **Everyone full access (⚠️ risky):**

  ```bash
  chmod 777 shared_dir  # -rwxrwxrwx
  ```

* **Directory only owner can enter/edit:**

  ```bash
  chmod 700 mydir       # drwx------
  ```

---

## 7. 🔢 Numeric (Octal) Permission Codes

Each permission digit = **sum of values**:

* `r` (read) = **4**
* `w` (write) = **2**
* `x` (execute) = **1**

So possible values:

| Number | Permission | Symbol                 |
| ------ | ---------- | ---------------------- |
| 0      | ---        | none                   |
| 1      | --x        | execute only           |
| 2      | -w-        | write only             |
| 3      | -wx        | write + execute        |
| 4      | r--        | read only              |
| 5      | r-x        | read + execute         |
| 6      | rw-        | read + write           |
| 7      | rwx        | read + write + execute |

---

### 3-Digit Format: `XYZ`

* **X = User (owner)**
* **Y = Group**
* **Z = Others**

👉 Example:

```bash
chmod 754 file.txt
```

\= `rwxr-xr--`

* User = `7` (rwx)
* Group = `5` (r-x)
* Others = `4` (r--)

---

### 🔥 Common Permission Codes

| Code | Meaning                         | Symbol      |
| ---- | ------------------------------- | ----------- |
| 777  | Full access for everyone        | `rwxrwxrwx` |
| 755  | Owner full, others read+execute | `rwxr-xr-x` |
| 700  | Only owner full                 | `rwx------` |
| 644  | Owner read/write, others read   | `rw-r--r--` |
| 600  | Only owner read/write           | `rw-------` |
| 444  | Everyone read-only              | `r--r--r--` |
| 000  | No one has permission           | `---------` |

---

### 4-Digit Codes (with Special Bits)

Sometimes you’ll see **4 digits** (like `4755`).
The **first digit** sets special modes:

* **4 = setuid** → run program as file owner
* **2 = setgid** → run program as group
* **1 = sticky bit** → only owner can delete files in a shared dir

👉 Example:

```bash
chmod 1777 /tmp
```

\= sticky bit set, so everyone can write, but only file owner can delete.

---

✅ Summary:

* 3 digits → basic permissions (`chmod 755`)
* 4 digits → includes special bits (`chmod 4755`)

---