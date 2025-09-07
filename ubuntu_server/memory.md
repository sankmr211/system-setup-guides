
# 1. TOP cmd

```
top
```

Perfect 👍 — that’s the header section of the **`top` command** output. Let’s break it line by line:

---

### 🟢 **1. System uptime & load**

```
top - 21:54:19 up 31 min,  2 users,  load average: 0.15, 0.59, 0.67
```

* **21:54:19** → Current system time.
* **up 31 min** → System has been running for 31 minutes (since last boot).
* **2 users** → Number of logged-in user sessions (e.g., terminals or SSH).
* **load average: 0.15, 0.59, 0.67** → Average number of tasks waiting for CPU over:

  * 1 minute → 0.15
  * 5 minutes → 0.59
  * 15 minutes → 0.67
    👉 On a single-core CPU, **load = 1.00 means fully busy**. Your values (<1) mean CPU is under low load.

---


mean the **average number of processes waiting for CPU (or I/O)** over time:

* **0.15** → last **1 minute**
* **0.59** → last **5 minutes**
* **0.67** → last **15 minutes**

---

### 🔎 How to read it

* If you have **1 CPU core** →

  * Load `1.00` = CPU fully busy.
  * Load `<1.00` = CPU has free capacity.
  * Load `>1.00` = CPU overloaded (too many tasks waiting).

* If you have **4 cores** →

  * Load `4.00` = fully busy.
  * Load `>4.00` = overloaded.

---

### ✅ In your case

* Values are **well below 1.0**.
* That means your system is **not under heavy CPU load** — it’s mostly idle and has capacity left.

---


Good question 👍 — there are several ways to check how many **CPU cores** your system has in Linux. Here are the most common:

---

### 🟢 1. Quick check with `nproc`

```bash
nproc
```

👉 Shows the number of available CPU cores.

---

### 🟢 2. Using `/proc/cpuinfo`

```bash
grep -c ^processor /proc/cpuinfo
```

👉 Counts how many `processor` entries exist → that equals the number of cores.

If you want details (model, MHz, etc.):

```bash
cat /proc/cpuinfo
```

---

### 🟢 3. Using `lscpu`

```bash
lscpu
```

👉 Gives a clean summary:

* CPU(s) → total cores
* Core(s) per socket
* Thread(s) per core
* Architecture, speed, etc.

---

### 🟢 4. With `htop` (interactive)

Run:

```bash
htop
```

👉 At the top, you’ll see CPU bars — one bar per core.

---

✅ Example:
If `nproc` prints `2`, that means you have **2 CPU cores**.

---


### 🔵 **2. Task summary**

```
Tasks: 124 total,   1 running, 123 sleeping,   0 stopped,   0 zombie
```

* **124 total** → Total processes running on system.
* **1 running** → Currently using CPU.
* **123 sleeping** → Idle but ready to run (waiting for input, network, etc.).
* **0 stopped** → None are paused/stopped.
* **0 zombie** → No zombie (dead but not cleaned up) processes. ✅ Good.

---

### 🟣 **3. CPU usage**

```
%Cpu(s):  0.2 us,  0.2 sy,  0.0 ni, 97.8 id,  1.8 wa,  0.0 hi,  0.0 si,  0.0 st
```

Breakdown of CPU time percentages:

* **0.2 us** → *User space* processes (apps you run).
* **0.2 sy** → *System/kernel* processes.
* **0.0 ni** → Processes with *nice* priority.
* **97.8 id** → CPU idle (not doing anything).
* **1.8 wa** → CPU waiting on I/O (disk/network).
* **0.0 hi** → Hardware interrupts.
* **0.0 si** → Software interrupts.
* **0.0 st** → Stolen time (if running in a VM).

👉 Interpretation: Your CPU is **almost idle (97.8%)**, with a tiny bit waiting on disk I/O (1.8%).

---

✅ **Summary for your snapshot**

* System has been up for **31 minutes**, with **2 active users**.
* CPU is mostly idle (plenty of room).
* Load average is **well below 1** → CPU is not overloaded.
* No zombie processes → system is healthy.

---

Got it 👍 — that’s the **Linux memory usage summary** (probably from `htop` or `free -m`). Let me explain each line:

---

### 🟢 **4. Memory (RAM)**

```
MiB Mem :   1474.2 total,    576.8 free,    509.3 used,    598.1 buff/cache
```

* **1474.2 total** → Total physical RAM = \~1.5 GB
* **576.8 free** → Memory not used at all right now (completely free).
* **509.3 used** → Memory actively used by running processes.
* **598.1 buff/cache** → Memory Linux is using for buffers (disk caching).

  * This looks "used", but it’s **reclaimable** if apps need it.

👉 So **actual available RAM** = free (576.8) + reclaimable buff/cache (598.1) ≈ **1175 MB (\~1.1 GB)**.

---

### 🔵 **Swap (virtual memory on disk)**

```
MiB Swap:   2048.0 total,   2047.7 free,      0.2 used.    964.9 avail Mem
```

* **2048.0 total** → Swap size = 2 GB.
* **2047.7 free** → Almost all swap is free.
* **0.2 used** → Just a few KB of swap is in use.
* **964.9 avail Mem** → Memory Linux estimates as *immediately available* for new applications without swapping.

---

### ✅ Interpretation

* Your system has **1.5 GB RAM + 2 GB Swap**.
* Around **509 MB is used**, **576 MB free**, **598 MB used for cache**.
* Swap is basically unused.
* So, your machine isn’t running out of memory — it still has \~1 GB usable RAM.

---

Great question 👍 — let’s break it down simply:

---

## 🔵 1. What is **Swap Memory**?

Swap is **virtual memory on disk** that Linux uses when your **RAM is full**.

* RAM = fast but limited (your system has \~1.5 GB).
* Swap = slow but large (on disk/SSD).
* Disk = slower, but larger.
* Swap prevents apps from crashing when RAM runs out.

So, when RAM gets crowded, the kernel **moves less-used data** from RAM → Swap, to free up RAM for active processes.

---

## ⚙️ How Swap Works

1. **Normal state (enough RAM)**

   * Programs run in RAM.
   * Swap stays mostly empty.

2. **RAM pressure (not enough RAM)**

   * The kernel finds data in RAM that hasn’t been used recently.
   * It copies that data to Swap (on disk).
   * This frees up RAM for active programs.

3. **Accessing swapped data**

   * If a program needs swapped-out data again → it must be read back from disk → this is **much slower** than RAM.

---

## 📊 Example with your system

* You have **1.5 GB RAM** and **2 GB Swap**.
* If programs demand **2.5 GB memory total**:

  * 1.5 GB stays in RAM.
  * Extra 1 GB spills into Swap.
  * System works, but **slows down** because disk I/O is slower.

---

## 🔧 Key points

* **Swap is a backup** for when RAM runs out, not a replacement.
* **Too much swap use = slow system** (like lag/freezes).
* Linux has a setting called **swappiness** (`/proc/sys/vm/swappiness`) → controls how aggressively it moves data to swap.

  * `0` → avoid swap as much as possible.
  * `60` (default) → balanced.
  * `100` → use swap a lot.

---

## ✅ In your case

* Swap = **2 GB**, almost free (so system is not stressed).
* If MySQL is crashing, it’s not because swap is missing, but probably because:

  * MySQL config uses too much memory **before kernel swaps**.
  * Or it’s failing due to other reasons (corruption, permissions, config).

---



## ⚙️ 2. How it works (example)

Let’s say your server has:

* **1.5 GB RAM**
* **2 GB Swap**

### Case A: Memory fits in RAM

* You open apps that use **1 GB RAM**.
* System has **0.5 GB free RAM left**, so **no swap is used**.

---

### Case B: RAM full, swap kicks in

* Now you open MySQL + browser + other apps → total **2 GB needed**.
* RAM = **1.5 GB full**.
* Extra **0.5 GB** spills into Swap.
* System keeps running, but accessing that 0.5 GB is **slower** (because it’s on disk).

---

### Case C: Heavy load (RAM + Swap full)

* If apps need **3.5 GB** total:

  * 1.5 GB → RAM
  * 2 GB → Swap
  * No memory left → system will **kill processes (OOM-killer)** to free memory.

---




