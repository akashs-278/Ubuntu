# Module 04 — Navigation & File Management Commands

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 03](./03-filesystem.md)** | **[Module 05 →](./05-text-search.md)**

---

## 📋 Table of Contents

- [4.1 Navigation Commands](#41-navigation-commands)
- [4.2 File Creation Commands](#42-file-creation-commands)
- [4.3 Directory Commands](#43-directory-commands)
- [4.4 Copying Files & Directories](#44-copying-files--directories)
- [4.5 Moving & Renaming](#45-moving--renaming)
- [4.6 Deleting Files & Directories](#46-deleting-files--directories)
- [4.7 Listing & Inspecting](#47-listing--inspecting)
- [4.8 Links (Hard & Symbolic)](#48-links-hard--symbolic)
- [4.9 Archive & Compression](#49-archive--compression)
- [4.10 Wildcards & Globbing](#410-wildcards--globbing)

---

## 4.1 Navigation Commands

### `pwd` — Print Working Directory
```bash
pwd
# Output: /home/akash/Documents
# Shows WHERE you currently are in the filesystem
```

### `cd` — Change Directory
```bash
cd /home/akash/Documents    # Go to absolute path
cd Documents                # Go to relative path (from current)
cd ..                       # Go up one directory (parent)
cd ../..                    # Go up two directories
cd ~                        # Go to home directory
cd -                        # Go back to previous directory
cd /                        # Go to root directory
```

### `ls` — List Directory Contents
```bash
ls                  # Basic listing
ls -l               # Long format (permissions, size, date)
ls -a               # Show hidden files (starting with .)
ls -la              # Long + hidden files (most useful!)
ls -lh              # Long + human-readable sizes (KB, MB, GB)
ls -lt              # Sort by modification time (newest first)
ls -lS              # Sort by file size (largest first)
ls -R               # Recursive — list all subdirectories
ls -d */            # List only directories
ls *.txt            # List only .txt files
ls /etc             # List a different directory without going there
```

**Understanding `ls -l` Output:**
```
-rw-r--r-- 1 akash akash 2048 Aug 20 10:30 myfile.txt
│         │ │     │     │    │           │
│         │ │     │     │    │           └── Filename
│         │ │     │     │    └── Last modified date
│         │ │     │     └── Size in bytes
│         │ │     └── Group owner
│         │ └── User owner
│         └── Link count
└── Permissions (type + owner + group + others)
```

---

## 4.2 File Creation Commands

### `touch` — Create Empty File
```bash
touch newfile.txt              # Create an empty file
touch file1.txt file2.txt      # Create multiple files at once
touch report_{1..5}.txt        # Creates report_1.txt to report_5.txt
touch {jan,feb,mar}_2024.txt   # Creates jan/feb/mar_2024.txt
touch -t 202401011200 file.txt # Set specific timestamp
```

### `echo` — Write Text to File
```bash
echo "Hello, Linux!" > file.txt    # Create/overwrite file
echo "More text" >> file.txt       # Append to file
printf "Name: %s\n" "Akash" > info.txt  # Formatted output
```

### `nano` / `vim` — Text Editors
```bash
nano newfile.txt   # Beginner-friendly editor
# Ctrl+O = save, Ctrl+X = exit, Ctrl+K = cut line

vim newfile.txt    # Powerful editor
# Press 'i' = insert mode, Esc = exit insert mode
# :w = save, :q = quit, :wq = save & quit, :q! = force quit
```

---

## 4.3 Directory Commands

### `mkdir` — Make Directory
```bash
mkdir myfolder                    # Create a directory
mkdir -p parent/child/grandchild  # Create nested dirs (no error if exists)
mkdir dir1 dir2 dir3              # Create multiple directories
mkdir -m 755 secured_folder       # Create with specific permissions

# Practical example — full project structure:
mkdir -p ~/Projects/webapp/{src,assets,tests,docs}
```

### `rmdir` — Remove Empty Directory
```bash
rmdir emptyfolder           # Remove empty directory
rmdir -p parent/child       # Remove empty dirs recursively upward
```

---

## 4.4 Copying Files & Directories

### `cp` — Copy Files
```bash
cp source.txt destination.txt      # Copy file with new name
cp file.txt /home/akash/backup/    # Copy to directory
cp *.txt /backup/                  # Copy all .txt files

cp -r sourcefolder/ destfolder/    # Copy directory (required for dirs)
cp -rp myfolder/ backup/           # Preserve permissions & timestamps
cp -a source/ destination/         # Archive mode (best for backups)
cp -v file.txt /backup/            # Verbose — show what's being copied
cp -i source.txt dest.txt          # Ask before overwrite
```

---

## 4.5 Moving & Renaming

### `mv` — Move or Rename
```bash
mv oldname.txt newname.txt          # Rename a file
mv olddir/ newdir/                  # Rename a directory

mv file.txt /home/akash/Documents/  # Move file
mv folder/ /home/akash/Documents/   # Move directory
mv *.log /var/log/archive/          # Move all log files

# Move & rename at the same time:
mv /tmp/draft.txt ~/Documents/final_report.txt

mv -i source.txt dest.txt           # Ask before overwrite
mv -n source.txt dest.txt           # Don't overwrite (no-clobber)
mv -v *.txt /backup/                # Verbose output
```

---

## 4.6 Deleting Files & Directories

### `rm` — Remove Files
```bash
rm file.txt                    # Delete a file
rm file1.txt file2.txt         # Delete multiple files
rm *.log                       # Delete all .log files

rm -r myfolder/                # Delete directory (with contents)
rm -rf myfolder/               # Force delete (no prompts)
rm -ri myfolder/               # Interactive delete (asks each file)
rm -v file.txt                 # Verbose — confirm each deletion
rm -i file.txt                 # Ask before each deletion
```

> ⚠️ **Danger: NEVER run:** `rm -rf /` or `rm -rf /*` — deletes your ENTIRE system!

**Safer Alternative — trash-cli:**
```bash
sudo apt install trash-cli
trash-put file.txt     # Move to trash (recoverable!)
trash-list             # Show trashed files
trash-restore          # Restore from trash
trash-empty            # Permanently empty trash
```

---

## 4.7 Listing & Inspecting

### `stat` — Detailed File Info
```bash
stat file.txt
# Shows: size, inode, permissions, access/modify/change times
```

### `file` — Detect File Type
```bash
file document.pdf       # → PDF document
file image.png          # → PNG image data
file script.sh          # → Bourne-Again shell script
file /bin/bash          # → ELF 64-bit executable
```

### `wc` — Word/Line Count
```bash
wc file.txt             # Lines, words, characters
wc -l file.txt          # Count lines only
wc -w file.txt          # Count words only
wc -c file.txt          # Count bytes
ls | wc -l              # Count files in directory
```

### `tree` — Visual Directory Tree
```bash
sudo apt install tree   # Install if needed
tree                    # Tree view of current directory
tree -L 2               # Only 2 levels deep
tree -d                 # Directories only
tree -a                 # Include hidden files
tree -h                 # Human-readable sizes
```

---

## 4.8 Links (Hard & Symbolic)

### Symbolic Links (Like Shortcuts)
```bash
ln -s /path/to/original linkname   # Create symbolic link
ln -s /usr/bin/python3 ~/python    # Shortcut to python3

ls -la ~/python   # Shows: python -> /usr/bin/python3

rm linkname       # Remove link (not the original)
```

### Hard Links
```bash
ln original.txt hardlink.txt    # Both point to same data
ls -li original.txt hardlink.txt # Same inode = same file
```

---

## 4.9 Archive & Compression

### `tar` — Tape Archive (Most Common)
```bash
# Create archives:
tar -czvf archive.tar.gz folder/   # Create .tar.gz (gzip)
tar -cjvf archive.tar.bz2 folder/  # Create .tar.bz2 (bzip2)

# Extract archives:
tar -xzvf archive.tar.gz           # Extract .tar.gz
tar -xvf archive.tar -C /dest/     # Extract to specific directory

# List contents (without extracting):
tar -tzvf archive.tar.gz

# Flags: c=create  x=extract  t=list  v=verbose  f=file  z=gzip  j=bzip2
```

### `zip` / `gzip`
```bash
zip -r archive.zip folder/          # Zip directory
unzip archive.zip -d /destination/  # Extract to directory
unzip -l archive.zip                # List without extracting

gzip file.txt                       # Compress → file.txt.gz
gunzip file.txt.gz                  # Decompress
gzip -k file.txt                    # Compress, keep original
```

---

## 4.10 Wildcards & Globbing

```bash
*          # Any number of any characters
?          # Exactly one character
[abc]      # One of: a, b, or c
[a-z]      # Any lowercase letter
[0-9]      # Any digit
{a,b,c}    # Exactly a, b, or c (brace expansion)

# Examples:
ls *.txt              # All .txt files
ls report_?.txt       # report_1.txt, report_2.txt...
ls *.{jpg,png,gif}    # All image files
rm *.tmp              # Delete all .tmp files
mkdir project_{1..5}  # Creates project_1 to project_5
touch file_{A..Z}.txt # Creates file_A.txt to file_Z.txt
```

---

## ✅ Module 04 Quick Reference

| Command | Purpose |
|---------|---------|
| `pwd` | Show current directory |
| `cd path` | Change directory |
| `ls -la` | List all files with details |
| `touch file` | Create empty file |
| `mkdir -p dir` | Create directory (with parents) |
| `cp -r src dst` | Copy directory recursively |
| `mv src dst` | Move or rename |
| `rm -rf dir` | Delete directory forcefully |
| `tar -czvf` | Create compressed archive |
| `tar -xzvf` | Extract compressed archive |
| `ln -s src link` | Create symbolic link |
| `tree -L 2` | Visual tree view (2 levels) |

---

> **▶ Next Module: [Module 05 — File Viewing, Search & Text Processing →](./05-text-search.md)**
