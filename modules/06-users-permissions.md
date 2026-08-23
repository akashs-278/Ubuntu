# Module 06 — Users, Groups & Permissions

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 05](./05-text-search.md)** | **[Module 07 →](./07-software-management.md)**

---

## 📋 Table of Contents

- [6.1 Linux User System](#61-linux-user-system)
- [6.2 User Management Commands](#62-user-management-commands)
- [6.3 Group Management](#63-group-management)
- [6.4 sudo & Root Privileges](#64-sudo--root-privileges)
- [6.5 File Permissions (rwx)](#65-file-permissions-rwx)
- [6.6 chmod — Change Permissions](#66-chmod--change-permissions)
- [6.7 chown & chgrp — Change Ownership](#67-chown--chgrp--change-ownership)
- [6.8 Special Permissions (SUID, SGID, Sticky Bit)](#68-special-permissions-suid-sgid-sticky-bit)
- [6.9 umask — Default Permissions](#69-umask--default-permissions)
- [6.10 Access Control Lists (ACL)](#610-access-control-lists-acl)

---

## 6.1 Linux User System

Linux is a **multi-user** operating system. Every file and process has an owner.

```
Types of Users:
┌─────────────────────────────────────────────────────┐
│  Root User (UID 0)                                  │
│  └── Superuser, has unlimited access to everything  │
│                                                     │
│  System Users (UID 1–999)                           │
│  └── Created by system/software (www-data, mysql)   │
│                                                     │
│  Regular Users (UID 1000+)                          │
│  └── Human users created at installation            │
└─────────────────────────────────────────────────────┘
```

### Important User Files
```bash
cat /etc/passwd    # All user accounts
# Format: username:x:UID:GID:comment:home:shell
# akash:x:1000:1000:Akash Kumar:/home/akash:/bin/bash

cat /etc/shadow    # Encrypted passwords (root only)
cat /etc/group     # All groups and members
cat /etc/gshadow   # Encrypted group passwords (root only)
```

### View Current User Info
```bash
whoami             # Print your username
id                 # UID, GID, and all groups you belong to
id akash           # Info for specific user
who                # Who is logged in right now
w                  # Who is logged in and what they're doing
last               # Login history
lastlog            # Last login time for all users
```

---

## 6.2 User Management Commands

### `useradd` / `adduser` — Create Users
```bash
# adduser (recommended — interactive, user-friendly):
sudo adduser john              # Create user "john" interactively

# useradd (low-level, manual):
sudo useradd john              # Create user (no home dir by default)
sudo useradd -m john           # Create user WITH home directory
sudo useradd -m -s /bin/bash john          # Set default shell
sudo useradd -m -g developers john         # Set primary group
sudo useradd -m -G sudo,docker john        # Add to supplementary groups
sudo useradd -m -e 2025-12-31 john         # Set account expiry date
sudo useradd -m -c "John Doe" john         # Add comment/full name

# Set password for new user:
sudo passwd john               # Set/change password for john
```

### `usermod` — Modify Users
```bash
sudo usermod -l newname oldname         # Rename a user
sudo usermod -d /new/home -m john       # Change home directory
sudo usermod -s /bin/zsh john           # Change shell
sudo usermod -aG sudo john              # Add to sudo group (admin)
sudo usermod -aG docker,www-data john   # Add to multiple groups
sudo usermod -L john                    # Lock user account
sudo usermod -U john                    # Unlock user account
sudo usermod -e 2025-12-31 john         # Set account expiry
sudo usermod -e "" john                 # Remove expiry (never expires)
```

### `userdel` — Delete Users
```bash
sudo userdel john              # Delete user (keeps home directory)
sudo userdel -r john           # Delete user AND home directory
sudo userdel -rf john          # Force delete even if logged in
```

### `passwd` — Manage Passwords
```bash
passwd                         # Change your own password
sudo passwd john               # Change another user's password
sudo passwd -l john            # Lock account (disable password)
sudo passwd -u john            # Unlock account
sudo passwd -e john            # Force password change on next login
sudo passwd -d john            # Delete password (no password required)
chage -l john                  # View password aging info
sudo chage -M 90 john          # Password expires after 90 days
sudo chage -W 7 john           # Warn 7 days before expiry
```

### Switching Users
```bash
su john                        # Switch to user john (requires john's password)
su -                           # Switch to root (requires root password)
su - john                      # Switch with full login environment
sudo su                        # Become root using YOUR password (if in sudo group)
exit                           # Return to previous user
```

---

## 6.3 Group Management

```bash
# View groups:
groups                          # Groups your current user belongs to
groups john                     # Groups john belongs to
cat /etc/group                  # All groups on the system

# Create a group:
sudo groupadd developers         # Create group "developers"
sudo groupadd -g 1500 devops     # Create group with specific GID

# Modify a group:
sudo groupmod -n newname oldname # Rename a group

# Delete a group:
sudo groupdel developers         # Delete a group

# Add user to group:
sudo usermod -aG groupname username    # -a = append (don't remove from other groups!)
sudo gpasswd -a john developers        # Alternative: add john to developers

# Remove user from group:
sudo gpasswd -d john developers        # Remove john from developers

# List members of a group:
getent group developers                # Show group and members
```

---

## 6.4 sudo & Root Privileges

### Using `sudo`
```bash
sudo command                    # Run command as root
sudo -u john command            # Run command as user john
sudo -i                         # Open root shell (interactive login)
sudo -s                         # Open root shell (non-login)
sudo !!                         # Re-run last command with sudo
sudo visudo                     # Safely edit /etc/sudoers file
```

### sudoers Configuration
```bash
sudo visudo                     # ALWAYS edit sudoers with this command!

# Format in /etc/sudoers:
# user  host=(runas)  commands
# john  ALL=(ALL:ALL)  ALL     → john can run any command as any user
# john  ALL=(ALL)  NOPASSWD: /usr/bin/apt  → run apt without password
```

### Understanding sudo vs su
| | `sudo command` | `su -` |
|-|---------------|--------|
| **Runs as** | Root (temporarily) | Switches TO root session |
| **Uses** | YOUR password | Root's password |
| **Logged** | Yes (in auth.log) | No |
| **Safer** | ✅ Yes | Less safe |

---

## 6.5 File Permissions (rwx)

Every file in Linux has **3 sets of permissions** for 3 entities:

```
-  rw-  r--  r--
│   │    │    │
│   │    │    └── Others (everyone else)
│   │    └─────── Group (file's group)
│   └──────────── Owner (file creator)
└──────────────── File type (- = file, d = dir, l = link)

Permission symbols:
  r = read    (4)
  w = write   (2)
  x = execute (1)
  - = no permission (0)
```

### Reading Permissions
```bash
ls -l myfile.txt
# -rwxr-xr-- 1 akash developers 1234 Aug 20 file.txt
#  │││└──┘└─┘
#  ││└─┘  └── Others: r-- (read only)
#  │└─┘────── Group:  r-x (read + execute)
#  └────────── Owner:  rwx (read + write + execute)
```

### Permission Values (Octal)
```
r = 4
w = 2
x = 1

Common combinations:
7 = rwx (4+2+1) = full permissions
6 = rw- (4+2)   = read + write
5 = r-x (4+1)   = read + execute
4 = r-- (4)     = read only
0 = --- (0)     = no permissions

Common permission sets:
755 = rwxr-xr-x → owner full, others read+execute (scripts, dirs)
644 = rw-r--r-- → owner rw, others read (regular files)
600 = rw------- → owner rw only (private files)
700 = rwx------ → owner only (private scripts)
777 = rwxrwxrwx → everyone full access (avoid in production!)
```

---

## 6.6 chmod — Change Permissions

### Numeric (Octal) Mode
```bash
chmod 755 script.sh            # rwxr-xr-x
chmod 644 file.txt             # rw-r--r--
chmod 600 private.key          # rw------- (SSH keys!)
chmod 777 shared.txt           # rwxrwxrwx (avoid this!)
chmod 000 file.txt             # ---------- (no access)
chmod -R 755 folder/           # Apply recursively to directory
```

### Symbolic Mode
```bash
chmod +x script.sh             # Add execute for all (owner+group+others)
chmod -x script.sh             # Remove execute for all
chmod +r file.txt              # Add read for all
chmod u+x script.sh            # Add execute for owner only (u=user/owner)
chmod g+w file.txt             # Add write for group
chmod o-r file.txt             # Remove read from others
chmod a+x script.sh            # Add execute for all (a=all)
chmod u=rwx,g=rx,o=r file.txt  # Set exact permissions
chmod go-rwx private.key       # Remove all from group and others
```

---

## 6.7 chown & chgrp — Change Ownership

### `chown` — Change File Owner
```bash
sudo chown john file.txt              # Change owner to john
sudo chown john:developers file.txt   # Change owner and group
sudo chown :developers file.txt       # Change only group (note the colon)
sudo chown -R john:john folder/       # Recursive — change entire directory
sudo chown --reference=ref.txt file.txt  # Copy ownership from another file
```

### `chgrp` — Change Group
```bash
sudo chgrp developers file.txt        # Change group to "developers"
sudo chgrp -R www-data /var/www/      # Recursive group change
```

---

## 6.8 Special Permissions (SUID, SGID, Sticky Bit)

### SUID — Set User ID (bit 4)
```bash
# File runs with OWNER's permissions (not executor's)
# Used by: /usr/bin/passwd, /usr/bin/sudo

chmod u+s file           # Set SUID
chmod 4755 file          # Numeric: 4 prefix = SUID
ls -l /usr/bin/passwd    # → -rwsr-xr-x (s in owner execute = SUID set)

# Find all SUID files (security audit):
find / -perm -4000 -type f 2>/dev/null
```

### SGID — Set Group ID (bit 2)
```bash
# On files: runs with GROUP's permissions
# On directories: new files inherit directory's group

chmod g+s directory/     # Set SGID on directory
chmod 2755 directory/    # Numeric: 2 prefix = SGID
ls -l                    # → drwxr-sr-x (s in group execute = SGID)

# Useful for shared team directories:
sudo mkdir /team
sudo chgrp developers /team
sudo chmod g+s /team     # All files created here belong to "developers"
```

### Sticky Bit (bit 1)
```bash
# On directories: users can only delete their OWN files
# Used on /tmp — everyone can write, but only delete own files

chmod +t /shared/         # Set sticky bit
chmod 1777 /shared/       # Numeric: 1 prefix = sticky bit
ls -ld /tmp               # → drwxrwxrwt (t = sticky bit set)
```

---

## 6.9 umask — Default Permissions

`umask` controls the **default permissions** for newly created files/directories.

```bash
umask               # View current umask (usually 0022)
umask 0022          # Set umask

# How umask works:
# Default max for files: 666 (rw-rw-rw-)
# Default max for dirs:  777 (rwxrwxrwx)
# umask 022 means: subtract 022
#   Files: 666 - 022 = 644 (rw-r--r--)
#   Dirs:  777 - 022 = 755 (rwxr-xr-x)

# Make umask permanent:
echo "umask 027" >> ~/.bashrc   # 027 = files: 640, dirs: 750
```

---

## 6.10 Access Control Lists (ACL)

ACLs allow **fine-grained permissions** beyond the basic owner/group/others model.

```bash
# Install ACL tools:
sudo apt install acl

# View ACL:
getfacl file.txt

# Set ACL — give john read+write on a file:
setfacl -m u:john:rw file.txt

# Set ACL for a group:
setfacl -m g:developers:rx folder/

# Set default ACL for directory (new files inherit it):
setfacl -d -m g:developers:rw shared/

# Remove ACL entry:
setfacl -x u:john file.txt

# Remove ALL ACLs:
setfacl -b file.txt
```

---

## ✅ Module 06 Quick Reference

| Command | Purpose |
|---------|---------|
| `id` | Show current user's UID, GID, groups |
| `sudo adduser name` | Create new user interactively |
| `sudo usermod -aG group user` | Add user to group |
| `sudo passwd user` | Change user password |
| `chmod 755 file` | Set permissions (numeric) |
| `chmod +x file` | Add execute permission |
| `chown user:group file` | Change owner and group |
| `ls -l` | View file permissions |
| `groups` | Show your groups |
| `sudo visudo` | Edit sudoers safely |
| `getfacl file` | View ACL permissions |
| `setfacl -m u:user:rw file` | Set ACL |

---

> **▶ Next Module: [Module 07 — Software Management →](./07-software-management.md)**
