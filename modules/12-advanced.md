# Module 12 — Advanced Commands & System Administration

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 11](./11-shell-scripting.md)**

---

## 📋 Table of Contents

- [12.1 System Administration Commands](#121-system-administration-commands)
- [12.2 Performance Tuning](#122-performance-tuning)
- [12.3 User & Authentication Management](#123-user--authentication-management)
- [12.4 File System Advanced](#124-file-system-advanced)
- [12.5 Network Administration](#125-network-administration)
- [12.6 Security Hardening](#126-security-hardening)
- [12.7 Kernel & Boot Management](#127-kernel--boot-management)
- [12.8 Virtualization & Containers (Docker)](#128-virtualization--containers-docker)
- [12.9 Text Processing Power Tools](#129-text-processing-power-tools)
- [12.10 Productivity & Miscellaneous](#1210-productivity--miscellaneous)

---

## 12.1 System Administration Commands

```bash
# System control:
sudo shutdown now              # Shutdown immediately
sudo shutdown -h +10           # Shutdown in 10 minutes
sudo shutdown -r now           # Reboot now
sudo reboot                    # Reboot
sudo halt                      # Halt system

# Power management:
systemctl suspend              # Suspend (sleep)
systemctl hibernate            # Hibernate
systemctl poweroff             # Power off

# System time:
timedatectl                    # Show time/date/timezone
sudo timedatectl set-time "2024-01-15 09:30:00"
sudo timedatectl set-timezone Asia/Kolkata
timedatectl list-timezones | grep India

# Sync hardware clock:
sudo hwclock --systohc          # System → Hardware clock
sudo hwclock --hctosys          # Hardware → System clock

# Kernel parameters (sysctl):
sysctl -a                       # All kernel parameters
sysctl net.ipv4.ip_forward      # Specific parameter
sudo sysctl -w net.ipv4.ip_forward=1   # Set temporarily
# Make permanent:
echo "net.ipv4.ip_forward=1" | sudo tee -a /etc/sysctl.conf
sudo sysctl -p                  # Apply /etc/sysctl.conf
```

---

## 12.2 Performance Tuning

```bash
# CPU information:
lscpu                           # CPU details
cat /proc/cpuinfo               # Detailed CPU info
nproc                           # Number of CPU cores

# CPU governor (performance mode):
cat /sys/devices/system/cpu/cpu0/cpufreq/scaling_governor
echo "performance" | sudo tee /sys/devices/system/cpu/cpu*/cpufreq/scaling_governor

# Memory:
free -h                         # RAM usage
cat /proc/meminfo               # Detailed memory
sudo sysctl vm.swappiness       # Swap aggressiveness (default=60)
sudo sysctl -w vm.swappiness=10 # Reduce swapping (better for desktops)

# I/O scheduler:
cat /sys/block/sda/queue/scheduler
echo mq-deadline | sudo tee /sys/block/sda/queue/scheduler

# OOM killer:
cat /proc/$$/oom_score           # OOM score of current process
echo -17 | sudo tee /proc/$(pgrep important_process)/oom_adj  # Protect process

# Benchmark disk I/O:
sudo apt install fio hdparm
sudo hdparm -tT /dev/sda        # Simple read benchmark

# Benchmark CPU:
sudo apt install sysbench
sysbench cpu run                 # CPU benchmark
sysbench memory run             # Memory benchmark
```

---

## 12.3 User & Authentication Management

```bash
# PAM (Pluggable Authentication Modules):
ls /etc/pam.d/                  # PAM config files
cat /etc/pam.d/common-auth      # Common auth settings

# Sudo configuration:
sudo visudo                     # Edit /etc/sudoers safely
cat /etc/sudoers.d/             # Sudoers drop-in directory

# Password policies:
sudo apt install libpam-pwquality
sudo nano /etc/security/pwquality.conf
# Set: minlen=12, dcredit=-1, ucredit=-1, lcredit=-1, ocredit=-1

# Account lockout (failed login protection):
sudo nano /etc/security/faillock.conf
# deny=5           → Lock after 5 failures
# unlock_time=300  → Auto-unlock after 5 minutes

# View failed logins:
sudo faillock --user akash      # Failed login attempts for user

# Login sessions:
who                             # Current sessions
w                               # Sessions + what they're doing
last                            # Login history
last -n 20                      # Last 20 logins
lastfail                        # Failed login attempts
sudo last -w /var/log/wtmp.1    # Previous month's logins

# Restrict SSH to key-only (no passwords):
sudo nano /etc/ssh/sshd_config
# Set: PasswordAuthentication no
#      PubkeyAuthentication yes
sudo systemctl restart ssh

# Two-factor authentication:
sudo apt install libpam-google-authenticator
google-authenticator             # Set up 2FA for your account
```

---

## 12.4 File System Advanced

```bash
# LVM (Logical Volume Manager):
sudo apt install lvm2
sudo pvcreate /dev/sdb                # Initialize physical volume
sudo vgcreate myvg /dev/sdb           # Create volume group
sudo lvcreate -L 10G -n mylv myvg     # Create 10GB logical volume
sudo mkfs.ext4 /dev/myvg/mylv         # Format
sudo mount /dev/myvg/mylv /mnt/data   # Mount

# Extend LV:
sudo lvextend -L +5G /dev/myvg/mylv   # Add 5GB
sudo resize2fs /dev/myvg/mylv         # Resize filesystem to match

# Quota (limit disk usage per user):
sudo apt install quota
sudo quotacheck -cum /home            # Initialize quota db
sudo quotaon /home                    # Enable quotas
sudo edquota -u akash                 # Edit user quota
sudo repquota /home                   # Report quota usage

# RAID status (if using software RAID):
cat /proc/mdstat
sudo mdadm --detail /dev/md0

# Check filesystem integrity:
sudo fsck /dev/sda1               # Run ONLY when unmounted!
sudo e2fsck -f /dev/sda1          # Force check ext4
sudo xfs_repair /dev/sda1         # Repair XFS filesystem

# Inotifywait — watch for file changes:
sudo apt install inotify-tools
inotifywait -m /etc               # Monitor /etc for changes
inotifywait -e modify file.txt    # Alert on file modification
```

---

## 12.5 Network Administration

```bash
# Network namespaces:
sudo ip netns list                # List namespaces
sudo ip netns add myns            # Create namespace
sudo ip netns exec myns bash      # Run commands in namespace

# Traffic shaping (tc):
sudo tc qdisc show dev eth0       # Show current queuing discipline
sudo tc qdisc add dev eth0 root tbf rate 1mbit burst 32kbit latency 400ms  # Limit to 1Mbps

# iptables (advanced firewall):
sudo iptables -L                  # List rules
sudo iptables -L -n -v            # Verbose with packets count
sudo iptables -A INPUT -p tcp --dport 80 -j ACCEPT     # Allow port 80
sudo iptables -A INPUT -s 10.0.0.1 -j DROP             # Block IP
sudo iptables -F                  # Flush (delete) all rules

# Save and restore iptables:
sudo iptables-save > /etc/iptables/rules.v4
sudo iptables-restore < /etc/iptables/rules.v4

# ip6tables — IPv6 firewall:
sudo ip6tables -L                 # List IPv6 rules

# Network performance:
sudo apt install nethogs
sudo nethogs eth0                 # Per-process bandwidth monitor

sudo apt install iftop
sudo iftop -i eth0                # Per-connection bandwidth monitor

sudo apt install bmon
bmon                              # Network bandwidth monitor

# VPN:
sudo apt install openvpn
sudo openvpn --config /path/to/config.ovpn   # Connect to VPN
```

---

## 12.6 Security Hardening

```bash
# Check for rootkits:
sudo apt install rkhunter chkrootkit
sudo rkhunter --update && sudo rkhunter --checkall
sudo chkrootkit

# File integrity monitoring:
sudo apt install aide
sudo aideinit                    # Initialize database
sudo aide --check                # Compare against baseline

# Check for SUID/SGID files (potential risks):
sudo find / -perm /4000 -type f 2>/dev/null   # SUID files
sudo find / -perm /2000 -type f 2>/dev/null   # SGID files

# Check for world-writable files:
sudo find / -perm -o+w -type f 2>/dev/null

# AppArmor — Mandatory Access Control:
sudo aa-status                   # AppArmor status
sudo aa-enforce /etc/apparmor.d/usr.sbin.nginx   # Enforce profile
sudo aa-complain /etc/apparmor.d/usr.sbin.nginx  # Complain mode (log only)

# Audit system:
sudo apt install auditd
sudo systemctl enable auditd --now
sudo ausearch -m avc             # Search for permission denials
sudo aureport --summary          # Summary report

# Open ports audit:
sudo ss -tulnp                   # What's listening?
sudo nmap -sV localhost          # Service versions on localhost

# SSH hardening (/etc/ssh/sshd_config):
# Port 2222                      # Non-standard port
# PermitRootLogin no             # Disable root login
# PasswordAuthentication no      # Key-only auth
# MaxAuthTries 3                 # Limit auth attempts
# AllowUsers akash               # Whitelist users
# Protocol 2                     # SSH protocol 2 only
```

---

## 12.7 Kernel & Boot Management

```bash
# Kernel version:
uname -r                         # Running kernel
uname -a                         # Full info

# List installed kernels:
dpkg --list | grep linux-image
ls /boot/vmlinuz*

# Remove old kernels:
sudo apt autoremove               # Removes unused kernels automatically

# Kernel modules:
lsmod                            # Loaded modules
modinfo modulename               # Module information
sudo modprobe modulename         # Load module
sudo modprobe -r modulename      # Unload module
echo "modulename" | sudo tee -a /etc/modules   # Load at boot

# GRUB bootloader:
cat /etc/default/grub            # GRUB settings
sudo update-grub                 # Update GRUB config
sudo nano /etc/default/grub      # Edit GRUB settings
# GRUB_DEFAULT=0                 → Default kernel entry
# GRUB_TIMEOUT=5                 → Boot menu timeout
# GRUB_CMDLINE_LINUX="..."       → Kernel boot parameters

# Boot into recovery mode:
# At GRUB menu → press 'e' → edit, find "quiet splash" → replace with "text"
# Then Ctrl+X to boot

# dmesg — Kernel messages:
dmesg                            # All kernel messages
dmesg | tail -20                 # Recent messages
dmesg | grep -i error            # Filter errors
dmesg | grep -i usb              # USB events
sudo dmesg -T                    # With timestamps
sudo dmesg -w                    # Follow (live kernel messages)
```

---

## 12.8 Virtualization & Containers (Docker)

### KVM (Kernel-based Virtual Machine)
```bash
# Check KVM support:
egrep -c '(vmx|svm)' /proc/cpuinfo    # >0 means CPU supports virtualization
kvm-ok                                  # Clear check (install: sudo apt install cpu-checker)

# Install KVM + tools:
sudo apt install qemu-kvm libvirt-daemon-system virt-manager

# Manage VMs:
virsh list --all                # List all VMs
virsh start myvm                # Start VM
virsh shutdown myvm             # Graceful shutdown
virsh destroy myvm              # Force stop
virsh suspend myvm              # Pause VM
virsh resume myvm               # Resume VM
virsh snapshot-create-as myvm snap1  # Create snapshot
```

### Docker
```bash
# Install Docker:
sudo apt install docker.io
sudo systemctl enable docker --now
sudo usermod -aG docker $USER   # Use Docker without sudo (re-login required)

# Basic Docker commands:
docker ps                       # Running containers
docker ps -a                    # All containers (including stopped)
docker images                   # List downloaded images
docker pull ubuntu:22.04        # Download Ubuntu image
docker run ubuntu:22.04 echo "hello"   # Run and execute command
docker run -it ubuntu:22.04 bash       # Interactive shell
docker run -d -p 8080:80 nginx  # Run nginx, map port 8080→80

# Container management:
docker start container_name
docker stop container_name
docker restart container_name
docker rm container_name        # Remove stopped container
docker rmi image_name           # Remove image

# Exec inside running container:
docker exec -it container_name bash

# Logs:
docker logs container_name
docker logs -f container_name   # Follow (live)

# Docker Compose:
sudo apt install docker-compose
docker-compose up -d            # Start services in background
docker-compose down             # Stop and remove containers
docker-compose logs -f          # Follow logs
```

---

## 12.9 Text Processing Power Tools

```bash
# jq — JSON processor:
sudo apt install jq
echo '{"name":"Akash","age":25}' | jq .
echo '{"name":"Akash"}' | jq '.name'         # Extract field
curl -s https://api.github.com/users/torvalds | jq '{name:.name, repos:.public_repos}'

# xargs — Build command from input:
find . -name "*.tmp" | xargs rm             # Delete all .tmp files
cat hosts.txt | xargs -I{} ping -c1 {}     # Ping each host
ls *.txt | xargs wc -l                     # Count lines in all .txt

# parallel — Run commands in parallel:
sudo apt install parallel
cat urls.txt | parallel wget               # Download all URLs in parallel
ls *.mp4 | parallel ffmpeg -i {} -c:a mp3 {.}.mp3  # Convert all videos

# column — Format into columns:
cat /etc/passwd | column -t -s:            # Format with : as separator
mount | column -t                          # Nicely formatted mount output

# paste — Merge lines of files:
paste file1.txt file2.txt                  # Side-by-side columns
paste -d, file1.txt file2.txt              # CSV output

# join — Join files on common field:
join sorted1.txt sorted2.txt

# tac — Reverse order of lines:
tac file.txt                               # Print file in reverse

# rev — Reverse characters in each line:
echo "hello" | rev                         # → olleh

# nl — Number lines:
nl file.txt                                # Like cat -n but better
nl -ba file.txt                            # Number all lines including blanks
```

---

## 12.10 Productivity & Miscellaneous

```bash
# man — Manual pages (most important learning tool!):
man ls                          # Manual for ls
man 5 crontab                   # Section 5 (file formats) of crontab
man -k keyword                  # Search all manuals
whatis ls                       # One-line description

# tldr — Simplified man pages:
sudo apt install tldr
tldr ls                         # Short, practical examples
tldr tar                        # Quick tar reference

# Clipboard from terminal:
sudo apt install xclip
cat file.txt | xclip -sel clip  # Copy file to clipboard
xclip -sel clip -o              # Paste from clipboard

# bc — Calculator:
echo "5 * 8.5" | bc
echo "scale=10; 355/113" | bc   # Pi approximation (10 decimal places)
bc                               # Interactive calculator

# date — Date & time:
date                             # Current date/time
date +"%Y-%m-%d"                # Just date (2024-08-23)
date +"%H:%M:%S"                # Just time (14:30:00)
date +"%A, %B %d %Y"            # Friday, August 23 2024
date -d "3 days ago"            # 3 days ago
date -d "next monday"           # Next Monday's date

# cal — Calendar:
cal                              # Current month
cal 2024                         # Full year 2024
cal 12 2024                      # December 2024

# time — Measure command duration:
time ls -la /home               # How long does ls take?
time ./my-script.sh             # Time your script

# script — Record terminal session:
script session.log               # Start recording
# ... do stuff ...
exit                             # Stop recording
scriptreplay timing.log session.log  # Replay recording

# notify-send — Desktop notifications:
notify-send "Build done!" "Your build completed successfully."
notify-send -u critical "ERROR" "Build failed!"

# zenity — GUI dialogs from terminal:
sudo apt install zenity
zenity --info --text="Task complete!"
answer=$(zenity --question --text="Continue?" && echo "yes" || echo "no")
name=$(zenity --entry --title="Input" --text="Enter your name:")

# xdg-open — Open file with default app:
xdg-open document.pdf           # Open PDF with default viewer
xdg-open https://google.com     # Open URL in default browser
xdg-open .                      # Open current folder in Files app
```

---

## ✅ Module 12 Quick Reference

| Command | Purpose |
|---------|---------|
| `sudo shutdown -r now` | Reboot immediately |
| `timedatectl` | Manage time/timezone |
| `sysctl -a` | Kernel parameters |
| `dmesg -T` | Kernel messages with timestamps |
| `sudo update-grub` | Update bootloader |
| `sudo rkhunter --checkall` | Check for rootkits |
| `virsh list --all` | List VMs |
| `docker ps -a` | List containers |
| `docker run -it ubuntu bash` | Interactive container |
| `jq '.field' file.json` | Parse JSON |
| `find . -name "*.tmp" \| xargs rm` | Find and delete |
| `man command` | Read the manual! |
| `tldr command` | Quick examples |
| `time command` | Measure duration |

---

> **🎉 Congratulations! You've completed all 12 modules!**
> 
> **🏠 [← Back to Main Course](../README.md)**
