# Module 09 — Process & System Management

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 08](./08-networking.md)** | **[Module 10 →](./10-shells.md)**

---

## 📋 Table of Contents

- [9.1 Understanding Processes](#91-understanding-processes)
- [9.2 Viewing Processes](#92-viewing-processes)
- [9.3 Managing Processes](#93-managing-processes)
- [9.4 Job Control (Background/Foreground)](#94-job-control-backgroundforeground)
- [9.5 System Monitoring](#95-system-monitoring)
- [9.6 Disk & Memory Management](#96-disk--memory-management)
- [9.7 System Services (systemd)](#97-system-services-systemd)
- [9.8 System Logs (journalctl)](#98-system-logs-journalctl)
- [9.9 Scheduling Tasks (cron & at)](#99-scheduling-tasks-cron--at)
- [9.10 System Information](#910-system-information)

---

## 9.1 Understanding Processes

```
Process States:
┌───────────────────────────────────────────────────┐
│  R  Running   — Currently using CPU               │
│  S  Sleeping  — Waiting for input/event           │
│  D  Sleeping  — Uninterruptible (disk I/O)        │
│  Z  Zombie    — Finished but not cleaned up       │
│  T  Stopped   — Paused (Ctrl+Z)                  │
└───────────────────────────────────────────────────┘

Every process has:
  PID   — Process ID (unique number)
  PPID  — Parent Process ID
  UID   — User who owns the process
  Nice  — Priority (-20 highest to 19 lowest)
```

---

## 9.2 Viewing Processes

```bash
# ps — Process Status (snapshot):
ps                              # Your current shell's processes
ps aux                          # ALL processes from ALL users (most useful)
ps aux | grep nginx             # Find a specific process
ps -ef                          # Full format listing
ps -u akash                     # All processes by user "akash"
ps --forest                     # Show process tree
ps -p 1234                      # Info for specific PID
ps -o pid,ppid,cmd,%mem,%cpu    # Custom output columns

# top — Interactive real-time monitor:
top
# Keys inside top:
# q = quit  | M = sort by memory  | P = sort by CPU
# k = kill  | r = renice (change priority)
# u = filter by user  | 1 = show individual CPUs
# h = help

# htop — Better interactive monitor (install first):
sudo apt install htop
htop
# Arrow keys to navigate, F9 to kill, F6 to sort, q to quit

# pgrep — Find PID by name:
pgrep nginx                     # PID of nginx processes
pgrep -u akash bash             # Bash PIDs owned by akash
pgrep -l firefox                # PID + name

# pidof — Get PID of running program:
pidof nginx                     # Returns PID(s)
pidof -x script.sh              # Including scripts
```

---

## 9.3 Managing Processes

### Killing Processes
```bash
# kill — Send signal to PID:
kill 1234                       # Send SIGTERM (15) — graceful shutdown
kill -9 1234                    # Send SIGKILL — force kill (no cleanup)
kill -15 1234                   # SIGTERM (graceful)
kill -l                         # List all signal names

# killall — Kill by name:
killall firefox                 # Kill all firefox processes
killall -9 nginx                # Force kill all nginx
killall -u akash                # Kill all processes by user

# pkill — Kill by pattern:
pkill firefox                   # Kill processes matching "firefox"
pkill -9 -u akash               # Force kill all of user akash's processes
pkill -f "python script.py"     # Kill process matching full command

# Common Signals:
# SIGTERM (15) — Polite request to terminate (default)
# SIGKILL (9)  — Force kill (cannot be ignored or caught)
# SIGHUP  (1)  — Hangup (reload config for daemons)
# SIGSTOP (19) — Pause process (like Ctrl+Z)
# SIGCONT (18) — Resume paused process
```

### Process Priority (Nice)
```bash
# nice — Start with priority (-20 highest, 19 lowest):
nice -n 10 ./my-script.sh       # Start with low priority (10)
nice -n -5 ./important.sh       # Higher priority (requires sudo for negative)
sudo nice -n -10 ./critical.sh  # High priority (root only for negative values)

# renice — Change priority of running process:
renice -n 5 -p 1234             # Change PID 1234 to priority 5
sudo renice -n -5 -p 1234       # Increase priority (root required)
renice -n 10 -u akash           # Lower priority for all user's processes
```

---

## 9.4 Job Control (Background/Foreground)

```bash
# Run command in background:
./long-script.sh &              # Run in background (& at end)
sleep 100 &                     # Run sleep in background

# List background jobs:
jobs                            # Show all background jobs
jobs -l                         # Show with PIDs

# Bring job to foreground:
fg                              # Bring last background job forward
fg %1                           # Bring job #1 to foreground
fg %2                           # Bring job #2 to foreground

# Send foreground job to background:
# First press Ctrl+Z to suspend (stop) it
Ctrl+Z                          # Pause/suspend running process
bg                              # Resume it in background
bg %1                           # Resume job #1 in background

# nohup — Keep running after logout:
nohup ./script.sh &             # Run and ignore hangup signal
nohup ./script.sh > output.log 2>&1 &  # With log file

# disown — Detach job from shell:
./long-script.sh &
disown %1                       # Detach so it keeps running if shell closes

# screen / tmux — Terminal multiplexers:
sudo apt install screen tmux
screen                          # Start screen session
screen -S mysession             # Named session
screen -ls                      # List sessions
screen -r mysession             # Reattach to session
# Inside screen: Ctrl+A then D = detach

tmux                            # Start tmux
tmux new -s mysession           # Named session
tmux ls                         # List sessions
tmux attach -t mysession        # Reattach
```

---

## 9.5 System Monitoring

```bash
# top / htop — Real-time process monitor (see 9.2)

# vmstat — Virtual memory stats:
vmstat                          # Snapshot
vmstat 1                        # Update every 1 second
vmstat 1 10                     # 10 snapshots, 1 second apart

# iostat — CPU and disk I/O stats:
sudo apt install sysstat
iostat                          # Snapshot
iostat -x 1                     # Extended, every 1 second
iostat -d 1 5                   # Disk stats, 5 snapshots

# sar — System Activity Reporter:
sar                             # CPU usage history
sar -u 1 5                      # CPU: 5 samples every 1 second
sar -r 1 5                      # Memory usage
sar -n DEV 1 5                  # Network stats

# watch — Run command repeatedly:
watch -n 2 df -h                # Update df every 2 seconds
watch -n 1 "ps aux | head -10"  # Update process list
watch -d free -h                # Highlight changes

# uptime — How long system has been running:
uptime
uptime -p                       # Human readable: "up 3 days, 4 hours"

# Load average explained (from uptime/top):
# 1.00 = 100% of 1 CPU core
# If you have 4 cores, load avg of 4.00 = 100% utilized
```

---

## 9.6 Disk & Memory Management

```bash
# Memory:
free                            # RAM usage
free -h                         # Human-readable (MB, GB)
free -m                         # In megabytes
cat /proc/meminfo               # Detailed memory info

# Disk Space:
df -h                           # All filesystems, human-readable
df -h /home                     # Specific partition
df -T                           # Show filesystem type

# Disk Usage by Directory:
du -sh /home/akash              # Size of directory
du -sh *                        # Size of each item here
du -ah . | sort -rh | head -20  # Top 20 largest items

# Find large files:
find / -type f -size +100M 2>/dev/null | sort -k5 -rn

# Block devices:
lsblk                           # List all disks and partitions
lsblk -f                        # Show filesystem types
sudo fdisk -l                   # Full disk info
sudo blkid                      # UUIDs of partitions

# Disk health:
sudo apt install smartmontools
sudo smartctl -a /dev/sda       # Full SMART health report
sudo smartctl -H /dev/sda       # Quick health check
```

---

## 9.7 System Services (systemd)

`systemd` manages all services in modern Ubuntu.

```bash
# Service status:
systemctl status nginx          # Status of nginx service
systemctl status ssh            # Status of SSH server
systemctl list-units            # All running units
systemctl list-units --type=service   # Only services
systemctl list-units --failed         # Failed services

# Start/Stop/Restart:
sudo systemctl start nginx      # Start service
sudo systemctl stop nginx       # Stop service
sudo systemctl restart nginx    # Stop then start
sudo systemctl reload nginx     # Reload config (no downtime)

# Enable/Disable (auto-start at boot):
sudo systemctl enable nginx     # Enable at boot
sudo systemctl disable nginx    # Disable at boot
sudo systemctl enable --now nginx   # Enable AND start immediately

# Mask/Unmask (completely prevent starting):
sudo systemctl mask nginx       # Prevent from starting
sudo systemctl unmask nginx     # Allow starting again

# View service logs:
journalctl -u nginx             # All nginx logs
journalctl -u nginx -f          # Follow nginx logs live
journalctl -u nginx --since "1 hour ago"

# systemd targets (like runlevels):
systemctl get-default           # Current default target
sudo systemctl set-default multi-user.target   # CLI only (no GUI)
sudo systemctl set-default graphical.target    # GUI mode
sudo systemctl isolate rescue.target           # Enter rescue mode
```

---

## 9.8 System Logs (journalctl)

```bash
journalctl                              # All system logs
journalctl -f                           # Follow (live view)
journalctl -n 50                        # Last 50 entries
journalctl -p err                       # Errors only
journalctl -p warning..err              # Warnings and errors
journalctl --since "2024-01-01"         # Since date
journalctl --since "1 hour ago"         # Since 1 hour ago
journalctl --since "09:00" --until "10:00"   # Time range
journalctl -u nginx                     # Specific service
journalctl -b                           # This boot session
journalctl -b -1                        # Previous boot
journalctl --disk-usage                 # How much space logs use
sudo journalctl --vacuum-size=100M      # Keep only last 100MB of logs
sudo journalctl --vacuum-time=2weeks    # Keep only last 2 weeks

# Traditional log files (still exist):
tail -f /var/log/syslog         # System log
tail -f /var/log/auth.log       # Authentication log (SSH, sudo)
tail -f /var/log/kern.log       # Kernel log
tail -f /var/log/dpkg.log       # Package install/remove log
```

---

## 9.9 Scheduling Tasks (cron & at)

### cron — Recurring Tasks
```bash
crontab -e                      # Edit your crontab (opens in editor)
crontab -l                      # List your crontab entries
crontab -r                      # Remove your crontab
sudo crontab -e -u akash        # Edit another user's crontab

# Crontab format:
# ┌─── minute (0-59)
# │ ┌── hour (0-23)
# │ │ ┌─ day of month (1-31)
# │ │ │ ┌ month (1-12)
# │ │ │ │ ┌ day of week (0-7, 0&7=Sunday)
# * * * * * /path/to/command

# Examples:
# */5 * * * * /script.sh         → Every 5 minutes
# 0 2 * * * /backup.sh           → Daily at 2:00 AM
# 0 9 * * 1 /weekly.sh           → Every Monday at 9:00 AM
# 0 0 1 * * /monthly.sh          → First day of each month at midnight
# @reboot /startup.sh            → Run once at system startup
# @daily /daily.sh               → Once per day (midnight)
# @weekly /weekly.sh             → Once per week

# System cron (edit as root):
sudo nano /etc/crontab
ls /etc/cron.d/                 # Per-application cron files
ls /etc/cron.daily/             # Scripts run daily
ls /etc/cron.hourly/            # Scripts run hourly
ls /etc/cron.weekly/            # Scripts run weekly
ls /etc/cron.monthly/           # Scripts run monthly
```

### at — One-Time Scheduled Task
```bash
sudo apt install at

at 10:30                        # Schedule at 10:30 AM today
at 10:30 tomorrow               # Tomorrow at 10:30
at now + 1 hour                 # 1 hour from now
at 3pm + 2 days                 # 3 PM, 2 days from now
# After entering 'at', type commands, then Ctrl+D to save

atq                             # List pending at jobs
atrm 3                          # Remove at job #3
```

---

## 9.10 System Information

```bash
# OS info:
uname -a                        # Full kernel + OS info
uname -r                        # Kernel version only
lsb_release -a                  # Ubuntu version details
cat /etc/os-release             # OS info file
hostnamectl                     # Hostname + OS + kernel

# Hardware info:
lscpu                           # CPU information
lsmem                           # Memory information
sudo lshw -short                # Full hardware summary
sudo lshw -class disk           # Disk info only
sudo lshw -class network        # Network hardware

# GPU:
lspci | grep -i vga             # Graphics card
nvidia-smi                      # NVIDIA GPU status (if installed)

# System uptime & load:
uptime                          # Uptime + load average
cat /proc/uptime                # Uptime in seconds

# Installed packages count:
dpkg -l | wc -l

# neofetch — Pretty system summary:
sudo apt install neofetch
neofetch

# inxi — Detailed hardware info:
sudo apt install inxi
inxi -Fxz                       # Full system info
inxi -N                         # Network info
inxi -D                         # Disk info
```

---

## ✅ Module 09 Quick Reference

| Command | Purpose |
|---------|---------|
| `ps aux` | Show all processes |
| `top` / `htop` | Real-time monitor |
| `kill -9 PID` | Force kill process |
| `killall name` | Kill by name |
| `jobs` | List background jobs |
| `fg` / `bg` | Foreground/background |
| `nohup cmd &` | Keep running after logout |
| `systemctl status svc` | Service status |
| `sudo systemctl start svc` | Start service |
| `sudo systemctl enable svc` | Enable at boot |
| `journalctl -u svc -f` | Follow service logs |
| `df -h` | Disk usage |
| `free -h` | Memory usage |
| `crontab -e` | Edit scheduled tasks |
| `uname -a` | System info |
| `neofetch` | Pretty system info |

---

> **▶ Next Module: [Module 10 — Shells in Ubuntu →](./10-shells.md)**
