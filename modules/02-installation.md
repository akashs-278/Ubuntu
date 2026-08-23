# Module 02 — Installing Ubuntu

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 01](./01-introduction.md)** | **[Module 03 →](./03-filesystem.md)**

---

## 📋 Table of Contents

- [2.1 Hardware Requirements](#21-hardware-requirements)
- [2.2 Downloading Ubuntu ISO](#22-downloading-ubuntu-iso)
- [2.3 Creating a Bootable USB](#23-creating-a-bootable-usb)
- [2.4 Method 1: Full Installation (Replace OS)](#24-method-1-full-installation-replace-os)
- [2.5 Method 2: Dual Boot (Ubuntu + Windows)](#25-method-2-dual-boot-ubuntu--windows)
- [2.6 Method 3: Virtual Machine (VirtualBox)](#26-method-3-virtual-machine-virtualbox)
- [2.7 Method 4: WSL2 on Windows](#27-method-4-wsl2-on-windows)
- [2.8 First Boot & Initial Setup](#28-first-boot--initial-setup)
- [2.9 Post-Installation Essentials](#29-post-installation-essentials)

---

## 2.1 Hardware Requirements

### Minimum Requirements

| Component | Minimum | Recommended |
|-----------|---------|-------------|
| **CPU** | 2 GHz dual-core | 2+ GHz quad-core |
| **RAM** | 4 GB | 8 GB or more |
| **Storage** | 25 GB | 50 GB or more |
| **Display** | 1024×768 | 1920×1080 |
| **USB Port** | Required | Required |
| **Internet** | Optional | Recommended |

> 💡 **For Server Edition:** 1 GB RAM and 10 GB storage are sufficient.

### Check Your Hardware (on existing Linux/Ubuntu)
```bash
# Check CPU info
lscpu

# Check RAM
free -h

# Check disk space
df -h

# Check all hardware info
sudo lshw -short
```

---

## 2.2 Downloading Ubuntu ISO

### Steps to Download:
1. Go to **[https://ubuntu.com/download](https://ubuntu.com/download)**
2. Choose **Ubuntu Desktop** (for personal use) or **Ubuntu Server**
3. Select the latest **LTS version** (e.g., 24.04 LTS)
4. Click **Download** — file is ~5 GB (`.iso` format)

### Verify Download Integrity (Important!)
```bash
# On Linux/Mac — verify SHA256 checksum
sha256sum ubuntu-24.04-desktop-amd64.iso

# Compare with the official hash on ubuntu.com
# If they match → download is safe!
```

### Download via Terminal (Linux/Mac)
```bash
# Using wget
wget https://releases.ubuntu.com/24.04/ubuntu-24.04.2-desktop-amd64.iso

# Using curl
curl -L -O https://releases.ubuntu.com/24.04/ubuntu-24.04.2-desktop-amd64.iso
```

---

## 2.3 Creating a Bootable USB

You need a **USB drive (8GB+)**. All data on the USB will be erased.

### Option A: Using Balena Etcher (Recommended — All OS)
```
1. Download Etcher: https://etcher.balena.io/
2. Open Etcher
3. Click "Flash from file" → select your .iso file
4. Click "Select target" → choose your USB drive
5. Click "Flash!" → wait ~10 minutes
6. Done! USB is bootable.
```

### Option B: Using `dd` Command (Linux Terminal)
```bash
# ⚠️ DANGER: This will erase your USB! Double-check the device name!

# Step 1: Find your USB device name
lsblk
# Look for your USB (e.g., /dev/sdb or /dev/sdc)

# Step 2: Unmount the USB (replace /dev/sdb with your device)
sudo umount /dev/sdb*

# Step 3: Write the ISO to USB
sudo dd if=ubuntu-24.04-desktop-amd64.iso of=/dev/sdb bs=4M status=progress oflag=sync

# ⚠️ IMPORTANT:
# - 'if' = input file (your ISO)
# - 'of' = output file (your USB device — NOT a partition like /dev/sdb1)
# - Never use /dev/sda — that is usually your main hard drive!
```

### Option C: Using `gnome-disks` (GUI on Ubuntu)
```
1. Open "Disks" application (gnome-disks)
2. Select your USB drive from the left panel
3. Click the ☰ menu → "Restore Disk Image"
4. Select your ISO file → click "Start Restoring"
```

---

## 2.4 Method 1: Full Installation (Replace OS)

> ⚠️ **WARNING:** This will erase everything on your computer. Back up your data first!

### Step-by-Step Installation:
```
1. Insert bootable USB → Restart computer
2. Press boot key (F12, F2, F9, Del, or Esc — depends on motherboard)
   Common boot keys:
   ─ Dell: F12
   ─ HP: F9
   ─ Lenovo: F12
   ─ ASUS: F8 or Esc
   ─ Acer: F12

3. In BIOS/Boot Menu:
   ─ Select USB drive as boot device
   ─ Disable "Secure Boot" if Ubuntu won't boot
   ─ Enable "Legacy Boot" if needed (older systems)

4. Ubuntu loads → Choose "Try or Install Ubuntu"

5. Installation Wizard:
   ┌──────────────────────────────────────────────┐
   │  Language  →  Select your language           │
   │  Keyboard  →  Select keyboard layout         │
   │  Connect   →  Optional WiFi connection       │
   │  Type      →  "Normal installation"          │
   │  Updates   →  "Install third-party software" │
   │  Disk      →  "Erase disk and install Ubuntu"│
   │  Timezone  →  Select your timezone           │
   │  Account   →  Enter name, username, password │
   │  Install!  →  Wait ~15-20 minutes            │
   └──────────────────────────────────────────────┘

6. Restart → Remove USB when prompted → Login!
```

---

## 2.5 Method 2: Dual Boot (Ubuntu + Windows)

> 💡 Keep Windows AND install Ubuntu. You choose which OS to boot each time.

### Prerequisites:
```bash
# On Windows — Free up space:
# 1. Open "Disk Management" (Windows + X → Disk Management)
# 2. Right-click C: drive → "Shrink Volume"
# 3. Shrink by at least 30,000 MB (30 GB) for Ubuntu

# 2. Disable Fast Startup in Windows:
# Control Panel → Power Options → "Choose what power buttons do"
# → Uncheck "Turn on fast startup" → Save

# 3. Disable Secure Boot in BIOS (if required)
```

### Installation Steps:
```
1. Boot from USB (same as Method 1, Steps 1-4)

2. At "Installation Type" screen:
   ─ Choose "Install Ubuntu alongside Windows Boot Manager"
   ─ OR choose "Something else" for manual partitioning

3. Manual Partition Setup (recommended):
   ┌──────────────────────────────────────────────┐
   │  Create partitions:                          │
   │  /         →  Root partition  (25+ GB, ext4) │
   │  swap       →  Swap space     (2× your RAM)  │
   │  /home      →  Home partition (remaining)    │
   └──────────────────────────────────────────────┘

4. Continue installation → Set up account

5. On reboot → GRUB menu appears:
   ─ Ubuntu (default)
   ─ Windows Boot Manager

6. Use arrow keys to select OS → Enter to boot
```

---

## 2.6 Method 3: Virtual Machine (VirtualBox)

> 💡 **Safest method for beginners** — Run Ubuntu inside Windows/Mac without any risk!

### Step 1: Install VirtualBox
```
1. Download: https://www.virtualbox.org/
2. Install VirtualBox (run installer as Administrator on Windows)
3. Also install "VirtualBox Extension Pack" for better performance
```

### Step 2: Create Ubuntu VM
```
1. Open VirtualBox → Click "New"
2. Settings:
   ─ Name: Ubuntu 24.04
   ─ Type: Linux
   ─ Version: Ubuntu (64-bit)

3. Memory: 
   ─ Minimum: 2048 MB (2 GB)
   ─ Recommended: 4096 MB (4 GB)

4. Hard Disk:
   ─ Create virtual disk
   ─ VDI format, Dynamically allocated
   ─ Size: 25–50 GB

5. Before starting:
   ─ Go to Settings → Storage
   ─ Click on "Empty" optical drive
   ─ Add your Ubuntu ISO file

6. Start VM → Install Ubuntu normally
```

### Step 3: Install VirtualBox Guest Additions (Better Performance)
```bash
# After Ubuntu is installed and running in VM:
# 1. In VirtualBox menu → Devices → "Insert Guest Additions CD Image"
# 2. In Ubuntu terminal, run:

sudo apt update
sudo apt install build-essential dkms linux-headers-$(uname -r)

# Then mount and run the Guest Additions installer:
sudo mount /dev/cdrom /mnt
sudo sh /mnt/VBoxLinuxAdditions.run
sudo reboot

# Now you get: shared clipboard, drag-and-drop, better resolution
```

---

## 2.7 Method 4: WSL2 on Windows

> 💡 **Fastest way** to start using Ubuntu on Windows 10/11 (no dual boot needed)!

### Install WSL2 (Windows Subsystem for Linux)
```powershell
# Open PowerShell as Administrator:

# Enable WSL
wsl --install

# Install Ubuntu specifically
wsl --install -d Ubuntu

# Set WSL2 as default
wsl --set-default-version 2

# List available distros
wsl --list --online

# Launch Ubuntu
wsl
```

### Managing WSL
```powershell
# Check WSL version
wsl --version

# List installed distros
wsl --list --verbose

# Shutdown WSL
wsl --shutdown

# Update WSL
wsl --update
```

---

## 2.8 First Boot & Initial Setup

After installation, your Ubuntu desktop appears. Here's what to do:

```bash
# 1. Update the system (ALWAYS do this first!)
sudo apt update && sudo apt upgrade -y

# 2. Check Ubuntu version
lsb_release -a
cat /etc/os-release

# 3. Check kernel version
uname -r
uname -a

# 4. Check system info
hostnamectl

# 5. Set timezone
timedatectl
sudo timedatectl set-timezone Asia/Kolkata   # Change to your timezone
timedatectl list-timezones                   # List all timezones

# 6. Set hostname (computer name)
sudo hostnamectl set-hostname my-ubuntu-pc
```

---

## 2.9 Post-Installation Essentials

Run these commands after every fresh Ubuntu install:

```bash
# ── STEP 1: Update System ─────────────────────────────
sudo apt update          # Refresh package lists
sudo apt upgrade -y      # Upgrade all packages
sudo apt dist-upgrade -y # Full system upgrade
sudo apt autoremove -y   # Remove unused packages

# ── STEP 2: Install Essential Tools ───────────────────
sudo apt install -y \
  curl wget git \
  vim nano \
  htop tree \
  net-tools \
  build-essential \
  software-properties-common \
  apt-transport-https \
  ca-certificates \
  gnupg \
  unzip zip \
  neofetch

# ── STEP 3: Install Snap (if not already) ─────────────
sudo apt install snapd

# ── STEP 4: Enable Firewall ───────────────────────────
sudo ufw enable
sudo ufw status

# ── STEP 5: Check System Info ─────────────────────────
neofetch          # Beautiful system info display
```

---

## ✅ Module 02 Summary

| Method | Best For | Risk Level |
|--------|----------|------------|
| Full Install | Dedicated Linux machine | High (erases disk) |
| Dual Boot | Keep Windows + use Linux | Medium |
| VirtualBox | Beginners testing Linux | Very Low (safe) |
| WSL2 | Windows users wanting Linux tools | None (safest) |

---

> **▶ Next Module: [Module 03 — Linux File System →](./03-filesystem.md)**
