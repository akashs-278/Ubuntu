# Ubuntu & Linux Master Commands Cheat Sheet

> **🏠 [← Back to Main Course](./README.md)**

A comprehensive reference of over 100+ essential Linux/Ubuntu commands organized by functional category. Each command includes its working principle, common options, and practical usage examples.

---

## 📋 Table of Contents

1. [Navigation & Directory Management](#1-navigation--directory-management)
2. [File Creation & Manipulation](#2-file-creation--manipulation)
3. [File Content Viewing & Search](#3-file-content-viewing--search)
4. [Text Processing (Grep, Sed, Awk)](#4-text-processing-grep-sed-awk)
5. [Permissions & Ownership](#5-permissions--ownership)
6. [User & Group Management](#6-user--group-management)
7. [Software & Package Management](#7-software--package-management)
8. [Networking & Remote Access](#8-networking--remote-access)
9. [Process & System Monitoring](#9-process--system-monitoring)
10. [Services & Systemd](#10-services--systemd)
11. [Disk & Filesystem Management](#11-disk--filesystem-management)
12. [Archive & Compression](#12-archive--compression)
13. [System Information & Hardware](#13-system-information--hardware)

---

## 1. Navigation & Directory Management

### `pwd` — Print Working Directory
* **Working Principle:** Queries the current process environment variable (`PWD`) or inspects the process working directory via system calls to output the exact absolute filesystem path of your current location.
```bash
pwd
```

### `cd` — Change Directory
* **Working Principle:** Executes the `chdir()` system call to change the current working directory of the active shell process.
```bash
cd /path/to/directory   # Navigate to absolute path
cd Documents            # Navigate to relative path
cd ..                   # Move up one directory level
cd ../..                # Move up two directory levels
cd ~                    # Move to user's home directory
cd -                    # Toggle back to the previous directory
```

### `ls` — List Directory Contents
* **Working Principle:** Reads directory entries using `opendir()` and `readdir()` system calls, retrieving file metadata (inodes) to display names, permissions, ownership, and sizes.
```bash
ls                      # List files in current directory
ls -l                   # Long listing format (permissions, size, owner, date)
ls -a                   # Include hidden files (starting with .)
ls -lh                  # Long format with human-readable file sizes (KB, MB, GB)
ls -lt                  # Sort by modification time (newest first)
ls -lS                  # Sort by file size (largest first)
ls -la /var/log         # Combine flags for specific directory
```

### `mkdir` — Make Directory
* **Working Principle:** Invokes the `mkdir()` system call to create new directory inodes on the target filesystem.
```bash
mkdir folder_name       # Create a new directory
mkdir dir1 dir2 dir3    # Create multiple directories at once
mkdir -p path/to/nested # Create nested parent and child directories recursively
```

---

## 2. File Creation & Manipulation

### `touch` — Create File / Update Timestamp
* **Working Principle:** Updates access and modification timestamps of a file. If the file does not exist, an empty file is created.
```bash
touch file.txt          # Create empty file or update timestamp
touch file1.txt file2.txt # Create multiple empty files
```

### `cp` — Copy Files & Directories
* **Working Principle:** Reads data from source file blocks and writes duplicate data to a new target inode location.
```bash
cp source.txt dest.txt     # Copy file to a new destination
cp file.txt /path/to/dir/  # Copy file into a directory
cp -r folder/ backup/      # Copy directory recursively (all contents)
cp -p file.txt backup.txt  # Preserve permissions and timestamps
```

### `mv` — Move or Rename
* **Working Principle:** If on the same filesystem partition, updates directory pointer links (inode reference) without moving actual disk data. If across partitions, copies data and deletes the source.
```bash
mv old_name.txt new_name.txt # Rename a file or directory
mv file.txt /path/to/dest/   # Move file to a different directory
```

### `rm` — Remove Files or Directories
* **Working Principle:** Unlinks file names from their inodes. When inode link count reaches 0, disk blocks are marked as free for overwriting.
```bash
rm file.txt             # Delete a file
rm -r folder/           # Remove directory and all its contents recursively
rm -rf folder/          # Force removal without interactive confirmation
```

---

## 3. File Content Viewing & Search

### `cat` — Concatenate & Print
```bash
cat file.txt            # Print contents of file to terminal
cat -n file.txt         # Print contents with line numbers
cat file1.txt file2.txt > merged.txt # Merge files into a new file
```

### `less` — Interactive Page Viewer
* **Working Principle:** Reads file into memory lazily (page-by-page), allowing bidirectional navigation without loading the entire file.
```bash
less /var/log/syslog    # View large file with scrolling (q to quit)
```

### `head` / `tail` — Beginning / End of File
```bash
head -n 10 file.txt     # View first 10 lines of a file
tail -n 10 file.txt     # View last 10 lines of a file
tail -f /var/log/syslog # Real-time continuous log streaming
```

### `find` — Search Files by Attributes
* **Working Principle:** Recursively walks directory trees searching for matching inode metadata (name, size, timestamp, permissions).
```bash
find . -name "*.txt"            # Find files by pattern in current dir
find /home -type f -size +50M   # Find regular files larger than 50MB
find . -mtime -7                # Find files modified in the last 7 days
find . -type f -name "*.tmp" -delete # Find and delete matching files
```

---

## 4. Text Processing (Grep, Sed, Awk)

### `grep` — Search Inside File Contents
* **Working Principle:** Scans text input line-by-line using regular expression matching engines to filter lines matching a pattern.
```bash
grep "error" /var/log/syslog    # Search for string in file
grep -i "ERROR" file.txt        # Case-insensitive search
grep -r "TODO" ./src/           # Recursive search across all subdirectories
grep -v "info" file.txt          # Invert match (show lines NOT matching)
grep -n "main" script.py        # Display matching line numbers
```

### `sed` — Stream Editor
* **Working Principle:** Reads input line-by-line into a pattern space buffer, applies transformation rules, and outputs the modified stream.
```bash
sed 's/old/new/g' file.txt      # Replace all occurrences of "old" with "new"
sed -i 's/foo/bar/g' file.txt   # Perform replacement in-place (modifies file)
sed -n '10,20p' file.txt        # Print lines 10 to 20 only
```

### `awk` — Pattern Scanning & Processing
* **Working Principle:** Splits each line into field variables (`$1`, `$2`...) based on a field separator and runs condition-action blocks.
```bash
awk '{print $1}' file.txt               # Print the first column of each line
awk -F: '{print $1, $6}' /etc/passwd    # Use ":" delimiter, print user & home dir
awk '$3 > 1000' /etc/passwd             # Filter lines where field 3 > 1000
```

---

## 5. Permissions & Ownership

### Permission Architecture (rwx)
- **r (read) = 4** | **w (write) = 2** | **x (execute) = 1**
- Entity sets: **User (u)** | **Group (g)** | **Others (o)**

### `chmod` — Change Access Permissions
```bash
chmod 755 script.sh     # User: rwx (7), Group: r-x (5), Others: r-x (5)
chmod 644 document.txt  # User: rw- (6), Group: r-- (4), Others: r-- (4)
chmod +x script.sh      # Add execute permission for everyone
chmod u+x script.sh     # Add execute permission for owner only
chmod -R 755 folder/    # Set permissions recursively for directory
```

### `chown` — Change File Owner & Group
```bash
sudo chown user file.txt            # Change owner
sudo chown user:group file.txt      # Change owner and group
sudo chown -R user:group folder/    # Change owner and group recursively
```

---

## 6. User & Group Management

```bash
sudo adduser john                   # Interactive creation of user account
sudo userdel -r john                # Delete user account and home directory
sudo usermod -aG sudo john          # Append user to administrative sudo group
passwd                              # Change current user password
sudo passwd john                    # Change specific user password
id                                  # Display current user UID, GID, and groups
whoami                              # Print effective username
```

---

## 7. Software & Package Management

### `apt` — Advanced Package Tool (Debian/Ubuntu)
```bash
sudo apt update                     # Refresh local index of available packages
sudo apt upgrade -y                 # Upgrade all installed packages
sudo apt install package_name       # Download and install package
sudo apt remove package_name        # Remove package (keep configuration)
sudo apt purge package_name         # Remove package and configuration files
sudo apt autoremove                 # Clean up orphan dependencies
apt search keyword                  # Search package repository
```

### `snap` — Containerized Package Manager
```bash
snap find package_name              # Search snap store
sudo snap install package_name      # Install snap package
sudo snap refresh                   # Update all snap packages
```

---

## 8. Networking & Remote Access

```bash
ip addr show                        # Display all network interfaces and IPs
ip route show                       # Display kernel IP routing table
ping -c 4 google.com                # Send ICMP echo requests to test reachability
traceroute google.com               # Trace network path hops to target
dig domain.com                      # DNS lookup for domain records
ss -tulnp                           # Show listening TCP/UDP sockets with process names
curl -O https://example.com/file    # Fetch/download file via HTTP/HTTPS
wget https://example.com/file.zip   # Download file directly from web
ssh user@remote_host                # Establish secure interactive SSH shell
scp file.txt user@host:/path/       # Copy file securely over SSH
rsync -avz local/ user@host:remote/ # Efficiently sync directories over SSH
sudo ufw status                     # View firewall status
sudo ufw allow 22/tcp               # Allow inbound traffic on port 22
```

---

## 9. Process & System Monitoring

```bash
ps aux                              # Snapshot of all running processes
top                                 # Interactive real-time process monitor
htop                                # Enhanced colorful process monitor
pgrep nginx                         # Find Process IDs by process name
kill PID                            # Send SIGTERM (15) for graceful termination
kill -9 PID                         # Send SIGKILL (9) to force terminate
killall process_name                # Terminate all processes by name
bg                                  # Resume suspended job in background
fg                                  # Bring background job to foreground
```

---

## 10. Services & Systemd

```bash
systemctl status service_name       # Check running state of a service
sudo systemctl start service_name   # Start service immediately
sudo systemctl stop service_name    # Stop running service
sudo systemctl restart service_name # Restart service
sudo systemctl enable service_name  # Enable service auto-start at boot
journalctl -u service_name -f       # Stream live logs for a specific service
```

---

## 11. Disk & Filesystem Management

```bash
df -h                               # Show filesystem disk space usage (human-readable)
du -sh /folder/path                 # Calculate total size of a directory
lsblk                               # List block devices (disks, partitions, mountpoints)
sudo fdisk -l                       # Display partition tables for attached drives
sudo mount /dev/sdb1 /mnt/usb       # Mount device partition to mount point
sudo umount /mnt/usb                # Unmount filesystem from directory
```

---

## 12. Archive & Compression

```bash
tar -czvf archive.tar.gz folder/    # Create gzip-compressed tar archive
tar -xzvf archive.tar.gz            # Extract gzip-compressed tar archive
zip -r archive.zip folder/          # Create zip archive recursively
unzip archive.zip                   # Extract zip archive
```

---

## 13. System Information & Hardware

```bash
uname -a                            # Display system kernel version and architecture
lsb_release -a                      # Display Ubuntu release distribution info
uptime                              # Show system uptime and load averages
free -h                             # Display total, used, and available RAM
lscpu                               # Display CPU hardware specifications
neofetch                            # Display visual system overview summary
```

---

> **🎉 End of Master Commands Cheat Sheet**
> 
> **🏠 [← Return to Course Main Page](./README.md)**