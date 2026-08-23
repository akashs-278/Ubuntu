# Module 01 — Introduction to Linux & Ubuntu

> **🏠 [← Back to Main Course](../README.md)** | **Next: [Module 02 — Installation →](./02-installation.md)**

---

## 📋 Table of Contents

- [1.1 What is an Operating System?](#11-what-is-an-operating-system)
- [1.2 What is Linux?](#12-what-is-linux)
- [1.3 History of Linux](#13-history-of-linux)
- [1.4 Linux Distributions Overview](#14-linux-distributions-overview)
- [1.5 What is Ubuntu?](#15-what-is-ubuntu)
- [1.6 Ubuntu Versions & LTS Releases](#16-ubuntu-versions--lts-releases)
- [1.7 Desktop vs Server Editions](#17-desktop-vs-server-editions)
- [1.8 Why Learn Linux/Ubuntu?](#18-why-learn-linuxubuntu)
- [1.9 The Linux Kernel Architecture](#19-the-linux-kernel-architecture)

---

## 1.1 What is an Operating System?

An **Operating System (OS)** is the core software that manages all hardware and software resources of a computer. It acts as the bridge between the user and the hardware.

```
┌─────────────────────────────────────────┐
│             User Applications           │
│  (Browser, Editor, Terminal, Games...)  │
├─────────────────────────────────────────┤
│          Operating System (OS)          │
│  (Linux Kernel + Shell + System Tools)  │
├─────────────────────────────────────────┤
│               Hardware                  │
│  (CPU, RAM, Disk, Network Card, GPU...) │
└─────────────────────────────────────────┘
```

**Key Functions of an OS:**
- Process Management (running programs)
- Memory Management (RAM allocation)
- File System Management (reading/writing files)
- Device Management (keyboard, mouse, disk)
- Security & Access Control
- Networking

---

## 1.2 What is Linux?

**Linux** is a free, open-source operating system kernel created by **Linus Torvalds** in 1991. It is:

| Property | Description |
|----------|-------------|
| **Open Source** | Source code is freely available and modifiable |
| **Multi-User** | Multiple users can use the system simultaneously |
| **Multi-Tasking** | Multiple processes run concurrently |
| **Portable** | Runs on phones, servers, desktops, routers, supercomputers |
| **Secure** | Strong permission model, fewer viruses |
| **Stable** | Known for high uptime (servers run for years) |
| **Free** | No license cost |

### Where is Linux Used?

```
🌐 Web Servers          → 96.3% of top web servers run Linux
📱 Android              → Android is built on the Linux kernel
☁️ Cloud Computing      → AWS, Google Cloud, Azure — all Linux
🔬 Supercomputers       → Top 500 supercomputers run Linux
🤖 Embedded Systems     → Routers, smart TVs, IoT devices
🚀 Space                → ISS, SpaceX use Linux
```

---

## 1.3 History of Linux

```
Timeline of Linux History
━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━

1969 ──► UNIX created at Bell Labs (Ken Thompson & Dennis Ritchie)
1983 ──► GNU Project started by Richard Stallman (free software)
1991 ──► Linus Torvalds writes Linux kernel v0.01 (age 21)
         Famous post: "I'm doing a (free) operating system"
1992 ──► Linux combined with GNU tools → complete OS
1994 ──► Linux kernel v1.0 released
1996 ──► Tux the penguin becomes Linux mascot
2003 ──► Red Hat Enterprise Linux gains enterprise adoption
2004 ──► Ubuntu 4.10 "Warty Warthog" — first Ubuntu release!
2007 ──► Android (Linux-based) launches on smartphones
2011 ──► Linux kernel v3.0 released
2019 ──► Microsoft uses Linux for Azure cloud
2022 ──► Linux kernel v6.0 released
2024 ──► Ubuntu 24.04 LTS "Noble Numbat" released

━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━━
```

> 💡 **Fun Fact:** "Linux" = **Lin**us + **Unix**. The name was actually chosen by the FTP server admin, not Linus himself!

---

## 1.4 Linux Distributions Overview

A **Linux Distribution (distro)** = Linux Kernel + GNU Tools + Package Manager + Desktop Environment + Applications.

```
              Linux Kernel
                  │
    ┌─────────────┼──────────────┐
    │             │              │
  Debian        Red Hat        Arch
    │           (RHEL)           │
    │             │              ├── Manjaro
    ├── Ubuntu    ├── Fedora     └── EndeavourOS
    │   │         └── CentOS
    │   ├── Linux Mint
    │   ├── Pop!_OS
    │   └── Elementary OS
    │
    └── Kali Linux
```

### Popular Distributions Compared

| Distro | Based On | Best For | Package Manager |
|--------|----------|----------|-----------------|
| **Ubuntu** | Debian | Beginners, General Use | APT (`apt`) |
| **Debian** | Independent | Servers, Stability | APT (`apt`) |
| **Fedora** | Red Hat | Developers, Latest Tech | DNF (`dnf`) |
| **CentOS/RHEL** | Red Hat | Enterprise Servers | YUM/DNF |
| **Arch Linux** | Independent | Advanced Users | Pacman (`pacman`) |
| **Kali Linux** | Debian | Cybersecurity | APT (`apt`) |
| **Mint** | Ubuntu | Windows Migrants | APT (`apt`) |

---

## 1.5 What is Ubuntu?

**Ubuntu** is a free, open-source Linux distribution developed by **Canonical Ltd.** (founded by Mark Shuttleworth). The name "Ubuntu" comes from the Nguni Bantu philosophy meaning:

> *"I am because we are"* — Ubuntu Philosophy

### Ubuntu Key Features

- 🧩 **User-Friendly** — Best Linux distro for beginners
- 🔄 **Regular Releases** — New version every 6 months
- 📦 **Huge Software Library** — Access to 50,000+ packages
- 🔐 **Security** — Automatic security updates
- 💰 **Free** — No cost for personal and commercial use
- 🌍 **Community** — Millions of users, huge support forums
- 🖥️ **GNOME Desktop** — Modern, clean interface (since Ubuntu 17.10)

---

## 1.6 Ubuntu Versions & LTS Releases

Ubuntu follows a predictable release schedule:

```
Release Cycle:
  Regular: Every 6 months (April & October)  — Supported 9 months
  LTS:     Every 2 years (April, even years) — Supported 5 years
```

### Version History (LTS Releases)

| Version | Codename | Release Date | Support Until |
|---------|----------|--------------|---------------|
| 20.04 LTS | Focal Fossa | April 2020 | April 2025 |
| 22.04 LTS | Jammy Jellyfish | April 2022 | April 2027 |
| **24.04 LTS** | **Noble Numbat** | **April 2024** | **April 2029** ✅ |
| 26.04 LTS | (upcoming) | April 2026 | April 2031 |

> 💡 **Recommendation for Beginners:** Always use the latest **LTS (Long Term Support)** version for maximum stability and security.

### How Ubuntu Versions Work
```
Ubuntu 24.04 LTS
        ↑  ↑  ↑
        │  │  └── Month (04 = April)
        │  └───── Year (2024)
        └──────── "LTS" = Long Term Support
```

---

## 1.7 Desktop vs Server Editions

| Feature | Ubuntu Desktop | Ubuntu Server |
|---------|---------------|---------------|
| **GUI** | GNOME Desktop (graphical) | No GUI (command-line only) |
| **Purpose** | Personal computing, development | Web servers, databases, cloud |
| **RAM Usage** | ~2–4 GB | ~512 MB – 1 GB |
| **ISO Size** | ~5 GB | ~1.5 GB |
| **Target User** | General users, developers | System administrators |
| **Default Apps** | Firefox, LibreOffice, Files | SSH server, basic utilities |

---

## 1.8 Why Learn Linux/Ubuntu?

```
💼 Career Opportunities:
   ─ DevOps Engineer
   ─ System Administrator
   ─ Cloud Engineer (AWS/GCP/Azure)
   ─ Backend Developer
   ─ Cybersecurity Analyst
   ─ Data Engineer / AI/ML Engineer

📈 Market Demand:
   ─ Linux skills = Higher salary
   ─ 90%+ of cloud infrastructure is Linux
   ─ All Docker containers run Linux
   ─ Kubernetes runs on Linux nodes

🛠️ Technical Benefits:
   ─ Full control over your system
   ─ Powerful command-line tools
   ─ Better for programming & development
   ─ Automation with shell scripts
```

---

## 1.9 The Linux Kernel Architecture

```
┌───────────────────────────────────────────────────┐
│                  USER SPACE                       │
│  ┌─────────┐  ┌─────────┐  ┌─────────────────┐   │
│  │  Shell  │  │   GUI   │  │  Applications   │   │
│  │ (bash)  │  │ (GNOME) │  │ (Firefox, etc.) │   │
│  └────┬────┘  └────┬────┘  └────────┬────────┘   │
│       │            │                │             │
│  ┌────▼────────────▼────────────────▼────────┐    │
│  │              System Calls (API)            │    │
│  └────────────────────┬──────────────────────┘    │
├───────────────────────┼───────────────────────────┤
│                  KERNEL SPACE                     │
│  ┌────────────────────▼──────────────────────┐    │
│  │           Linux Kernel                    │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐  │    │
│  │  │ Process  │ │ Memory   │ │   File   │  │    │
│  │  │ Manager  │ │ Manager  │ │  System  │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘  │    │
│  │  ┌──────────┐ ┌──────────┐ ┌──────────┐  │    │
│  │  │ Network  │ │ Security │ │  Device  │  │    │
│  │  │  Stack   │ │ (SELinux)│ │ Drivers  │  │    │
│  │  └──────────┘ └──────────┘ └──────────┘  │    │
│  └───────────────────────────────────────────┘    │
├───────────────────────────────────────────────────┤
│                   HARDWARE                        │
│      CPU     RAM     Disk     Network     GPU      │
└───────────────────────────────────────────────────┘
```

---

## ✅ Module 01 Summary

| Topic | Key Takeaway |
|-------|-------------|
| OS | Software layer between user and hardware |
| Linux | Free, open-source OS kernel by Linus Torvalds (1991) |
| Distros | Many Linux variants; Ubuntu is most beginner-friendly |
| Ubuntu | Canonical's user-friendly distro, released every 6 months |
| LTS | Stable releases supported for 5 years — best for beginners |
| Desktop/Server | Desktop has GUI; Server is CLI-only |

---

> **▶ Next Module: [Module 02 — Installation Guide →](./02-installation.md)**
