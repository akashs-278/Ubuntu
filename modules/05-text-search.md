# Module 05 — File Viewing, Search & Text Processing

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 04](./04-navigation-files.md)** | **[Module 06 →](./06-users-permissions.md)**

---

## 📋 Table of Contents

- [5.1 Viewing File Contents](#51-viewing-file-contents)
- [5.2 Searching Files — find](#52-searching-files--find)
- [5.3 Searching Inside Files — grep](#53-searching-inside-files--grep)
- [5.4 Text Processing — cut, sort, uniq, tr](#54-text-processing--cut-sort-uniq-tr)
- [5.5 Stream Editor — sed](#55-stream-editor--sed)
- [5.6 Text Patterns — awk](#56-text-patterns--awk)
- [5.7 Pipelines & Redirection](#57-pipelines--redirection)
- [5.8 Comparing Files — diff, comm, cmp](#58-comparing-files--diff-comm-cmp)
- [5.9 locate & which & whereis](#59-locate--which--whereis)

---

## 5.1 Viewing File Contents

### `cat` — Concatenate & Display
```bash
cat file.txt              # Print file contents to terminal
cat -n file.txt           # Print with line numbers
cat -A file.txt           # Show special characters (tabs, line endings)
cat file1.txt file2.txt   # Concatenate and display multiple files
cat file1.txt file2.txt > combined.txt  # Merge files into one

# Create a file by typing (Ctrl+D to finish):
cat > newfile.txt
```

### `less` — Page-by-Page Viewer (Recommended)
```bash
less file.txt             # Open file in pager (scroll up and down)
less +G file.txt          # Open at the END of file
less +/pattern file.txt   # Open and search for pattern

# less keyboard shortcuts:
# Space / f    → Next page
# b            → Previous page
# g            → Go to beginning
# G            → Go to end
# /pattern     → Search forward
# ?pattern     → Search backward
# n            → Next search result
# N            → Previous search result
# q            → Quit
```

### `more` — Basic Pager
```bash
more file.txt             # View page by page (only forward scrolling)
# Space = next page, q = quit, / = search
```

### `head` — Show First Lines
```bash
head file.txt             # First 10 lines (default)
head -n 20 file.txt       # First 20 lines
head -n -5 file.txt       # All lines EXCEPT the last 5
head -c 100 file.txt      # First 100 bytes/characters
head *.log                # First 10 lines of each .log file
```

### `tail` — Show Last Lines
```bash
tail file.txt             # Last 10 lines (default)
tail -n 20 file.txt       # Last 20 lines
tail -n +5 file.txt       # All lines FROM line 5 onwards
tail -c 200 file.txt      # Last 200 bytes

# LIVE log monitoring (most useful feature!):
tail -f /var/log/syslog            # Watch log file in real-time
tail -f /var/log/auth.log          # Watch authentication log live
tail -f app.log | grep ERROR       # Live filter for errors only
```

### `tac` — Reverse `cat` (Print Backwards)
```bash
tac file.txt              # Print file with lines in reverse order
```

---

## 5.2 Searching Files — `find`

`find` is one of the most powerful commands in Linux.

```bash
# ── Basic Syntax ─────────────────────────────────────
# find [where] [what criteria] [action]

# ── Find by Name ─────────────────────────────────────
find . -name "file.txt"           # Find file.txt in current dir
find /home -name "*.log"          # Find all .log files in /home
find / -name "passwd"             # Find file named "passwd" system-wide
find . -iname "readme.md"         # Case-insensitive name search

# ── Find by Type ─────────────────────────────────────
find . -type f                    # Find only regular files
find . -type d                    # Find only directories
find . -type l                    # Find only symbolic links

# ── Find by Size ─────────────────────────────────────
find . -size +100M                # Files larger than 100 MB
find . -size -1k                  # Files smaller than 1 KB
find . -size 50M                  # Files exactly 50 MB
find /var -size +50M -type f      # Large files in /var

# ── Find by Time ─────────────────────────────────────
find . -mtime -7                  # Modified in last 7 days
find . -mtime +30                 # Modified more than 30 days ago
find . -newer reference.txt       # Files newer than reference.txt
find . -atime -1                  # Accessed in last 24 hours

# ── Find by Permissions ──────────────────────────────
find . -perm 777                  # Files with exact 777 permissions
find . -perm /222                 # Files with any write permission
find . -perm -u+x                 # Files executable by owner

# ── Find by Owner ────────────────────────────────────
find /home -user akash            # Files owned by user "akash"
find . -group developers          # Files owned by group "developers"

# ── Find & Execute Actions ───────────────────────────
find . -name "*.tmp" -delete              # Find and delete .tmp files
find . -name "*.txt" -exec cat {} \;      # Find and print each file
find . -name "*.sh" -exec chmod +x {} \; # Find and make scripts executable
find . -type f -exec ls -lh {} \;         # Find files and list details

# ── Useful Combinations ──────────────────────────────
find / -name "*.conf" -type f 2>/dev/null           # Suppress permission errors
find /home -name "*.mp4" -size +500M -type f        # Large videos
find . -empty                                       # Find empty files/dirs
find . -name "*.py" | xargs grep "import os"        # Find Python files using os module
```

---

## 5.3 Searching Inside Files — `grep`

`grep` searches for **patterns inside file contents**.

```bash
# ── Basic Usage ──────────────────────────────────────
grep "pattern" file.txt           # Search for pattern in file
grep "error" /var/log/syslog      # Find errors in syslog
grep "root" /etc/passwd           # Find lines with "root"

# ── Case & Matching Options ──────────────────────────
grep -i "ERROR" file.txt          # Case-insensitive search
grep -v "error" file.txt          # Invert: show lines WITHOUT pattern
grep -w "cat" file.txt            # Whole word match (not "catch")
grep -x "exact line" file.txt     # Whole line match

# ── Output Control ───────────────────────────────────
grep -n "pattern" file.txt        # Show line numbers
grep -c "pattern" file.txt        # Count matching lines
grep -l "pattern" *.txt           # Only show filenames (not content)
grep -o "pattern" file.txt        # Print only the matched part
grep -A 3 "pattern" file.txt      # 3 lines AFTER match
grep -B 3 "pattern" file.txt      # 3 lines BEFORE match
grep -C 3 "pattern" file.txt      # 3 lines AROUND match (context)

# ── Recursive Search ─────────────────────────────────
grep -r "pattern" /etc/           # Search recursively in /etc
grep -r "TODO" ~/Projects/        # Find all TODOs in project
grep -rl "error" /var/log/        # List files containing "error"
grep -rn "function main" *.go     # Find function in Go files

# ── Regular Expressions ──────────────────────────────
grep "^root" /etc/passwd          # Lines STARTING with "root"
grep "bash$" /etc/passwd          # Lines ENDING with "bash"
grep -E "error|warning" file.txt  # Extended regex: error OR warning
grep -E "[0-9]{3}" file.txt       # Lines with 3 consecutive digits
grep -P "\d{4}-\d{2}-\d{2}" log   # Perl regex: date format

# ── Practical Examples ───────────────────────────────
grep -r "password" /etc/ 2>/dev/null        # Find password references
ps aux | grep nginx                          # Find nginx process
history | grep "apt install"                 # Find past installs
cat access.log | grep "404"                  # Find 404 errors
```

---

## 5.4 Text Processing — `cut`, `sort`, `uniq`, `tr`

### `cut` — Extract Columns/Fields
```bash
cut -d: -f1 /etc/passwd           # Extract field 1, delimiter ":"
cut -d: -f1,3 /etc/passwd         # Fields 1 and 3
cut -c1-10 file.txt               # First 10 characters per line
cut -c5- file.txt                 # From character 5 to end
cut -d, -f2 data.csv              # Column 2 of CSV file
```

### `sort` — Sort Lines
```bash
sort file.txt                     # Sort alphabetically
sort -r file.txt                  # Reverse sort (Z to A)
sort -n numbers.txt               # Numeric sort
sort -rn numbers.txt              # Reverse numeric sort
sort -u file.txt                  # Sort and remove duplicates
sort -k2 data.txt                 # Sort by 2nd field/column
sort -t: -k3 -n /etc/passwd       # Sort /etc/passwd by UID
sort -h sizes.txt                 # Human-readable sort (10K, 2M, 1G)
```

### `uniq` — Remove Duplicates
```bash
# Must be used after sort (uniq only removes adjacent duplicates)
sort file.txt | uniq              # Remove duplicate lines
sort file.txt | uniq -c           # Count occurrences
sort file.txt | uniq -d           # Show only duplicated lines
sort file.txt | uniq -u           # Show only unique lines
```

### `tr` — Translate/Replace Characters
```bash
echo "hello" | tr 'a-z' 'A-Z'    # Convert to uppercase
echo "HELLO" | tr 'A-Z' 'a-z'    # Convert to lowercase
echo "hello world" | tr ' ' '_'  # Replace spaces with underscores
echo "hello" | tr -d 'l'         # Delete character 'l'
echo "aabbcc" | tr -s 'a-z'      # Squeeze repeated chars
cat file.txt | tr -d '\r'        # Remove Windows carriage returns
```

---

## 5.5 Stream Editor — `sed`

`sed` edits text non-interactively — great for scripts.

```bash
# ── Substitute (Replace) ─────────────────────────────
sed 's/old/new/' file.txt         # Replace first occurrence per line
sed 's/old/new/g' file.txt        # Replace ALL occurrences (global)
sed 's/old/new/i' file.txt        # Case-insensitive replace
sed 's/old/new/2' file.txt        # Replace 2nd occurrence only

# Edit file IN PLACE (modify the actual file):
sed -i 's/old/new/g' file.txt     # Replace and save to file
sed -i.bak 's/old/new/g' file.txt # Replace and keep backup as .bak

# ── Print Specific Lines ─────────────────────────────
sed -n '5p' file.txt              # Print only line 5
sed -n '5,10p' file.txt           # Print lines 5 to 10
sed -n '/pattern/p' file.txt      # Print lines matching pattern

# ── Delete Lines ─────────────────────────────────────
sed '5d' file.txt                 # Delete line 5
sed '5,10d' file.txt              # Delete lines 5 to 10
sed '/pattern/d' file.txt         # Delete lines matching pattern
sed '/^$/d' file.txt              # Delete blank lines
sed '/^#/d' file.txt              # Delete comment lines

# ── Insert / Append ──────────────────────────────────
sed '3i\New line here' file.txt   # Insert before line 3
sed '3a\New line here' file.txt   # Append after line 3

# ── Practical Examples ───────────────────────────────
sed -i 's/http:/https:/g' urls.txt              # Upgrade URLs to HTTPS
sed -i '/^#/d;/^$/d' config.conf                # Remove comments and blank lines
sed -n '1~2p' file.txt                          # Print odd-numbered lines
```

---

## 5.6 Text Patterns — `awk`

`awk` is a full programming language for text processing.

```bash
# ── Basic Syntax ─────────────────────────────────────
# awk 'pattern { action }' file

# Print all lines:
awk '{print}' file.txt            # Print every line
awk '{print $1}' file.txt         # Print first field (word)
awk '{print $1, $3}' file.txt     # Print fields 1 and 3
awk '{print NR, $0}' file.txt     # Print with line numbers

# ── Field Separator ──────────────────────────────────
awk -F: '{print $1}' /etc/passwd  # Use ":" as delimiter, print field 1
awk -F, '{print $2}' data.csv     # Parse CSV, print column 2
awk -F'\t' '{print $3}' data.tsv  # Tab-separated values

# ── Conditions ───────────────────────────────────────
awk '$3 > 1000' /etc/passwd       # Lines where field 3 > 1000 (UID)
awk '/pattern/ {print $0}' file   # Print lines matching pattern
awk 'NR==5' file.txt              # Print only line 5
awk 'NR>=5 && NR<=10' file.txt    # Print lines 5 to 10

# ── Calculations ─────────────────────────────────────
awk '{sum += $1} END {print sum}' numbers.txt   # Sum of column 1
awk 'END {print NR}' file.txt                   # Count total lines
awk '{count[$1]++} END {for (w in count) print count[w], w}' file.txt  # Word freq

# ── Practical Examples ───────────────────────────────
awk -F: '$3 >= 1000 {print $1, $3}' /etc/passwd  # Show non-system users
df -h | awk '{print $1, $5}'                      # Disk usage percentage
ps aux | awk '{print $1, $11}' | head -10         # Process owner & name
```

---

## 5.7 Pipelines & Redirection

### Output Redirection
```bash
command > file.txt          # Redirect stdout to file (overwrite)
command >> file.txt         # Redirect stdout to file (append)
command 2> errors.txt       # Redirect stderr to file
command 2>&1                # Redirect stderr to stdout
command > output.txt 2>&1   # Both stdout and stderr to file
command &> output.txt       # Shorthand for both stdout+stderr
command > /dev/null         # Discard output (silence)
command 2>/dev/null         # Discard errors only
```

### Input Redirection
```bash
command < input.txt         # Read from file instead of keyboard
sort < unsorted.txt > sorted.txt   # Read from file, write to file
```

### Pipes `|` — Chain Commands
```bash
# The pipe | sends output of one command as input to next

ls -la | grep ".txt"                      # List, filter .txt files
cat /etc/passwd | cut -d: -f1 | sort     # List sorted usernames
ps aux | grep nginx                       # Find nginx process
df -h | grep -v tmpfs                     # Disk usage without tmpfs
history | tail -20                        # Last 20 commands
cat file.txt | wc -l                      # Count lines in file
cat access.log | grep "404" | wc -l      # Count 404 errors
ls -lS | head -10                        # 10 largest files
du -ah . | sort -rh | head -20           # Top 20 largest items
```

### `tee` — Split Output to File AND Terminal
```bash
command | tee output.txt             # Show output AND save to file
command | tee -a output.txt          # Append instead of overwrite
make build 2>&1 | tee build.log      # Save build output to log
```

### Here Documents
```bash
cat > script.sh << 'EOF'
#!/bin/bash
echo "This is inside a heredoc"
echo "Useful for multi-line content"
EOF
```

---

## 5.8 Comparing Files — `diff`, `comm`, `cmp`

### `diff` — Show Differences Between Files
```bash
diff file1.txt file2.txt          # Show differences (line by line)
diff -u file1.txt file2.txt       # Unified format (like git diff)
diff -i file1.txt file2.txt       # Case-insensitive comparison
diff -r dir1/ dir2/               # Compare two directories
diff --color file1.txt file2.txt  # Colorized output (newer diff)
```

### `cmp` — Byte-by-Byte Comparison
```bash
cmp file1.txt file2.txt           # Find first difference (byte level)
cmp -s file1.txt file2.txt        # Silent: exit code 0 = identical
```

### `comm` — Common/Different Lines Between Sorted Files
```bash
comm sorted1.txt sorted2.txt      # 3 columns: only in 1 | only in 2 | common
comm -12 sorted1.txt sorted2.txt  # Only lines in BOTH files
comm -23 sorted1.txt sorted2.txt  # Only lines unique to file1
```

---

## 5.9 `locate`, `which`, `whereis`

```bash
# locate — Find files by name (uses database, very fast)
locate filename.txt               # Find file anywhere
locate "*.conf"                   # Find all .conf files
sudo updatedb                     # Update locate database
locate -i "readme"                # Case-insensitive search

# which — Find location of a command
which python3                     # → /usr/bin/python3
which bash                        # → /usr/bin/bash
which ls                          # → /usr/bin/ls

# whereis — Find binary, source, and man page of command
whereis bash                      # → bash: /usr/bin/bash /usr/share/man/man1/bash.1.gz
whereis python3
```

---

## ✅ Module 05 Quick Reference

| Command | Purpose |
|---------|---------|
| `cat file` | Display file contents |
| `less file` | Page-by-page viewer |
| `head -n N file` | First N lines |
| `tail -f file` | Live log monitoring |
| `find . -name "*.txt"` | Find files by name |
| `grep -r "pattern" dir` | Search text in files |
| `sed 's/old/new/g'` | Find and replace in text |
| `awk -F: '{print $1}'` | Extract columns |
| `sort \| uniq -c` | Count unique occurrences |
| `cut -d: -f1` | Extract fields |
| `diff file1 file2` | Compare files |
| `cmd \| tee file` | Show output and save |

---

> **▶ Next Module: [Module 06 — Users, Groups & Permissions →](./06-users-permissions.md)**
