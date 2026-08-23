# Module 07 — Software Management

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 06](./06-users-permissions.md)** | **[Module 08 →](./08-networking.md)**

---

## 📋 Table of Contents

- [7.1 Package Management Overview](#71-package-management-overview)
- [7.2 APT — Advanced Package Tool](#72-apt--advanced-package-tool)
- [7.3 dpkg — Low-Level Package Manager](#73-dpkg--low-level-package-manager)
- [7.4 Snap Packages](#74-snap-packages)
- [7.5 Flatpak](#75-flatpak)
- [7.6 AppImage](#76-appimage)
- [7.7 Compiling from Source](#77-compiling-from-source)
- [7.8 Managing Repositories (PPA)](#78-managing-repositories-ppa)
- [7.9 System Update & Cleanup](#79-system-update--cleanup)

---

## 7.1 Package Management Overview

```
Software Installation Methods in Ubuntu:
┌────────────────────────────────────────────────────────┐
│  APT (apt)    → Official Ubuntu packages (recommended) │
│  dpkg         → Low-level .deb package installer       │
│  Snap         → Canonical's sandboxed packages         │
│  Flatpak      → Universal Linux app format             │
│  AppImage     → Portable, no-install executables       │
│  Source code  → Compile manually (maximum control)     │
└────────────────────────────────────────────────────────┘
```

---

## 7.2 APT — Advanced Package Tool

APT is the **primary** package manager for Ubuntu/Debian systems.

### Updating Package Lists & System
```bash
sudo apt update                 # Refresh package lists from repositories
sudo apt upgrade                # Upgrade all installed packages
sudo apt full-upgrade           # Upgrade + remove conflicting old packages
sudo apt dist-upgrade           # Smart upgrade (adds/removes as needed)

# Update and upgrade in one command:
sudo apt update && sudo apt upgrade -y
```

### Installing Packages
```bash
sudo apt install nginx                    # Install nginx web server
sudo apt install git vim curl wget        # Install multiple packages at once
sudo apt install -y nginx                 # Auto-confirm (no prompts)
sudo apt install ./package.deb            # Install local .deb file via apt
sudo apt install --reinstall nginx        # Reinstall (fix broken install)
sudo apt install python3=3.10.*           # Install specific version
```

### Removing Packages
```bash
sudo apt remove nginx            # Remove package (keep config files)
sudo apt purge nginx             # Remove package AND config files
sudo apt autoremove              # Remove unused dependency packages
sudo apt autoremove --purge      # Remove unused deps + their configs
```

### Searching Packages
```bash
apt search nginx                 # Search for packages by name/description
apt list                         # List all available packages
apt list --installed             # List only installed packages
apt list --upgradable            # List packages with available updates
apt show nginx                   # Show detailed package info
```

### Package Information
```bash
apt show nginx                   # Version, size, dependencies, description
apt depends nginx                # List what nginx depends on
apt rdepends nginx               # What packages depend on nginx
dpkg -L nginx                    # List all files installed by nginx
```

### Fixing Broken Packages
```bash
sudo apt --fix-broken install    # Fix broken/incomplete installations
sudo apt -f install              # Same (short form)
sudo dpkg --configure -a         # Configure all unconfigured packages
```

---

## 7.3 dpkg — Low-Level Package Manager

```bash
# Install a .deb file:
sudo dpkg -i package.deb              # Install local .deb file
sudo dpkg -i --force-overwrite pkg.deb # Force overwrite conflicts

# Remove:
sudo dpkg -r packagename              # Remove (keep configs)
sudo dpkg -P packagename              # Purge (remove + configs)

# Query:
dpkg -l                               # List all installed packages
dpkg -l | grep nginx                  # Search installed packages
dpkg -l "linux-*"                     # List Linux kernel packages
dpkg -s nginx                         # Show package status/info
dpkg -L nginx                         # List files installed by package
dpkg -S /usr/bin/python3              # Which package owns this file?

# Check package status:
dpkg --get-selections                 # All installed packages
dpkg --get-selections | grep -v deinstall  # Only installed ones
```

---

## 7.4 Snap Packages

Snap is Canonical's **sandboxed** application format — isolated from the system.

```bash
# Basic snap commands:
snap find firefox                   # Search for snap packages
snap install firefox                # Install firefox snap
snap install --classic code         # Install in classic mode (less sandboxed)
snap remove firefox                 # Remove snap
snap list                           # List installed snaps
snap info firefox                   # Detailed snap info
snap refresh firefox                # Update a specific snap
snap refresh                        # Update all snaps

# Managing snap services:
snap start firefox                  # Start a snap
snap stop firefox                   # Stop a snap
snap restart firefox                # Restart a snap
snap enable firefox                 # Enable snap (auto-start)
snap disable firefox                # Disable snap

# Snap channels (stable/beta/edge):
snap install --channel=beta firefox  # Install beta version
snap switch --channel=stable firefox # Switch to stable channel

# Snap permissions:
snap connections firefox            # View snap's connections/permissions
snap connect firefox:home           # Allow access to home directory
snap disconnect firefox:home        # Remove permission

# View snap logs:
snap logs firefox
```

---

## 7.5 Flatpak

Flatpak is a **universal** Linux package format that works across distros.

```bash
# Install Flatpak:
sudo apt install flatpak
sudo apt install gnome-software-plugin-flatpak

# Add Flathub repository (main Flatpak app store):
flatpak remote-add --if-not-exists flathub https://flathub.org/repo/flathub.flatpakrepo

# Restart system after setup.

# Search for apps:
flatpak search spotify

# Install:
flatpak install flathub com.spotify.Client
flatpak install flathub org.gimp.GIMP

# Run an app:
flatpak run com.spotify.Client

# List installed:
flatpak list

# Update all Flatpak apps:
flatpak update

# Remove:
flatpak uninstall com.spotify.Client

# Remove unused runtimes:
flatpak uninstall --unused
```

---

## 7.6 AppImage

AppImage is a **portable** format — no installation needed!

```bash
# Download an AppImage (example: Obsidian):
wget https://example.com/app.AppImage

# Make it executable:
chmod +x app.AppImage

# Run it:
./app.AppImage

# Optional: Move to /usr/local/bin for system-wide access:
sudo mv app.AppImage /usr/local/bin/myapp
myapp   # Now runs from anywhere

# AppImageLauncher — manage AppImages automatically:
sudo apt install appimagelauncher
```

---

## 7.7 Compiling from Source

When a package isn't available via apt/snap/flatpak, compile from source:

```bash
# Step 1: Install build tools
sudo apt install build-essential git autoconf libtool

# Step 2: Download source code
git clone https://github.com/example/project.git
cd project

# Step 3: Read the README or INSTALL file!
cat README.md
cat INSTALL

# Step 4: Configure the build (checks dependencies)
./configure
./configure --prefix=/usr/local    # Install to /usr/local instead of /usr

# Step 5: Compile
make                               # Compile using all CPU cores
make -j$(nproc)                    # Parallel build (faster!)

# Step 6: Install
sudo make install

# Uninstall (if supported):
sudo make uninstall

# Common pattern for CMake projects:
mkdir build && cd build
cmake ..
make -j$(nproc)
sudo make install
```

---

## 7.8 Managing Repositories (PPA)

A **PPA (Personal Package Archive)** lets you add third-party software sources.

```bash
# Add a PPA:
sudo add-apt-repository ppa:ondrej/php    # PHP PPA
sudo apt update                            # Refresh after adding

# Remove a PPA:
sudo add-apt-repository --remove ppa:ondrej/php

# View all repositories:
cat /etc/apt/sources.list
ls /etc/apt/sources.list.d/

# Add repository manually (e.g., Docker):
# 1. Add GPG key:
curl -fsSL https://download.docker.com/linux/ubuntu/gpg | sudo gpg --dearmor -o /etc/apt/keyrings/docker.gpg

# 2. Add repository:
echo "deb [arch=amd64 signed-by=/etc/apt/keyrings/docker.gpg] https://download.docker.com/linux/ubuntu $(lsb_release -cs) stable" | sudo tee /etc/apt/sources.list.d/docker.list

# 3. Update and install:
sudo apt update && sudo apt install docker-ce

# Pin a package (prevent automatic upgrade):
sudo apt-mark hold nginx
sudo apt-mark unhold nginx
apt-mark showhold              # Show pinned packages
```

---

## 7.9 System Update & Cleanup

```bash
# Full system update workflow:
sudo apt update                  # Refresh package lists
sudo apt upgrade -y              # Upgrade all packages
sudo apt dist-upgrade -y         # Full upgrade (handles dependencies)
sudo apt autoremove -y           # Remove orphaned packages
sudo apt autoclean               # Clear old downloaded package files
sudo apt clean                   # Clear ALL downloaded package files

# View apt history:
cat /var/log/apt/history.log
grep "Install" /var/log/apt/history.log | tail -20

# Check disk space used by apt cache:
du -sh /var/cache/apt/archives/

# Unattended upgrades (auto security updates):
sudo apt install unattended-upgrades
sudo dpkg-reconfigure unattended-upgrades

# List recently installed packages:
grep "install" /var/log/dpkg.log | tail -20

# Check package integrity:
sudo apt install debsums
sudo debsums nginx               # Verify nginx file checksums
```

---

## ✅ Module 07 Quick Reference

| Command | Purpose |
|---------|---------|
| `sudo apt update` | Refresh package lists |
| `sudo apt upgrade -y` | Upgrade all packages |
| `sudo apt install pkg` | Install a package |
| `sudo apt remove pkg` | Remove a package |
| `sudo apt purge pkg` | Remove + config files |
| `sudo apt autoremove` | Remove orphaned packages |
| `apt search name` | Search for packages |
| `apt show pkg` | Package details |
| `dpkg -l` | List installed packages |
| `dpkg -S /path/file` | Which package owns file |
| `snap install pkg` | Install snap package |
| `flatpak install flathub app` | Install Flatpak app |

---

> **▶ Next Module: [Module 08 — Networking Commands →](./08-networking.md)**
