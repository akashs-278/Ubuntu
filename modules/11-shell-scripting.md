# Module 11 — Shell Scripting (Bash)

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 10](./10-shells.md)** | **[Module 12 →](./12-advanced.md)**

---

## 📋 Table of Contents

- [11.1 Your First Script](#111-your-first-script)
- [11.2 Variables & Data Types](#112-variables--data-types)
- [11.3 User Input](#113-user-input)
- [11.4 Conditionals (if/elif/else)](#114-conditionals-ifelifelse)
- [11.5 Test Conditions & Operators](#115-test-conditions--operators)
- [11.6 Loops](#116-loops)
- [11.7 Functions](#117-functions)
- [11.8 Arrays](#118-arrays)
- [11.9 String Operations](#119-string-operations)
- [11.10 File Operations in Scripts](#1110-file-operations-in-scripts)
- [11.11 Exit Codes & Error Handling](#1111-exit-codes--error-handling)
- [11.12 Practical Script Examples](#1112-practical-script-examples)

---

## 11.1 Your First Script

```bash
# Step 1: Create the script file
nano hello.sh

# Step 2: Write the script:
#!/bin/bash
# This is a comment
# Every script starts with a shebang line: #!/bin/bash

echo "Hello, World!"
echo "Today is: $(date)"
echo "You are: $USER"

# Step 3: Make it executable
chmod +x hello.sh

# Step 4: Run it
./hello.sh

# Or run without execute permission:
bash hello.sh
```

### Shebang Line
```bash
#!/bin/bash      # Use Bash specifically
#!/usr/bin/env bash  # More portable (finds bash in PATH)
#!/bin/sh        # POSIX sh (most portable, fewer features)
#!/usr/bin/python3   # Python script
```

---

## 11.2 Variables & Data Types

```bash
#!/bin/bash

# Variable assignment (NO spaces around =)
name="Akash"
age=25
city="Bangalore"

# Accessing variables (use $):
echo $name
echo "My name is $name, age $age"
echo "I live in ${city}"        # {} for clarity

# Command output as variable:
current_date=$(date)
file_count=$(ls | wc -l)
echo "Date: $current_date"
echo "Files: $file_count"

# Constants (readonly):
readonly PI=3.14159
readonly MAX_RETRIES=3

# Arithmetic (only integers in bash):
a=10
b=3
echo $((a + b))     # 13
echo $((a - b))     # 7
echo $((a * b))     # 30
echo $((a / b))     # 3
echo $((a % b))     # 1 (remainder)
echo $((a ** 2))    # 100 (power)

# For decimals, use bc:
echo "scale=2; 10 / 3" | bc      # 3.33
echo "scale=4; sqrt(2)" | bc -l  # 1.4142
```

---

## 11.3 User Input

```bash
#!/bin/bash

# Basic input:
echo "What is your name?"
read name
echo "Hello, $name!"

# Prompt on same line:
read -p "Enter your age: " age
echo "You are $age years old"

# Silent input (for passwords):
read -sp "Enter password: " password
echo ""  # Newline after silent input
echo "Password received (${#password} chars)"

# Input with timeout:
read -t 5 -p "Enter within 5 seconds: " input

# Read multiple values:
read -p "Enter first and last name: " first last
echo "First: $first, Last: $last"

# Read into array:
read -a colors -p "Enter colors (space-separated): "
echo "First color: ${colors[0]}"
```

---

## 11.4 Conditionals (if/elif/else)

```bash
#!/bin/bash

age=20

# Basic if:
if [ $age -ge 18 ]; then
    echo "Adult"
fi

# if/else:
if [ $age -ge 18 ]; then
    echo "Adult"
else
    echo "Minor"
fi

# if/elif/else:
if [ $age -lt 13 ]; then
    echo "Child"
elif [ $age -lt 18 ]; then
    echo "Teenager"
elif [ $age -lt 65 ]; then
    echo "Adult"
else
    echo "Senior"
fi

# One-liner (and/or):
[ $age -ge 18 ] && echo "Adult" || echo "Minor"

# case statement (like switch):
read -p "Enter OS: " os
case $os in
    ubuntu|debian)
        echo "Debian-based"
        ;;
    fedora|centos|rhel)
        echo "Red Hat-based"
        ;;
    arch|manjaro)
        echo "Arch-based"
        ;;
    *)
        echo "Unknown OS"
        ;;
esac
```

---

## 11.5 Test Conditions & Operators

```bash
# Numeric comparisons:
[ $a -eq $b ]    # equal
[ $a -ne $b ]    # not equal
[ $a -lt $b ]    # less than
[ $a -le $b ]    # less than or equal
[ $a -gt $b ]    # greater than
[ $a -ge $b ]    # greater than or equal

# String comparisons:
[ "$a" = "$b" ]     # equal (use quotes!)
[ "$a" != "$b" ]    # not equal
[ -z "$a" ]         # empty string
[ -n "$a" ]         # non-empty string
[[ "$a" =~ ^[0-9]+$ ]]   # regex match (Bash only)

# File tests:
[ -f file ]      # file exists and is regular file
[ -d dir ]       # directory exists
[ -e path ]      # file/dir exists (any type)
[ -r file ]      # file is readable
[ -w file ]      # file is writable
[ -x file ]      # file is executable
[ -s file ]      # file exists and is NOT empty
[ -L file ]      # file is a symbolic link
[ file1 -nt file2 ]   # file1 is newer than file2
[ file1 -ot file2 ]   # file1 is older than file2

# Logical operators:
[ cond1 ] && [ cond2 ]   # AND
[ cond1 ] || [ cond2 ]   # OR
[ ! condition ]           # NOT

# [[ ]] — Extended test (Bash only, preferred):
[[ $name == "Akash" ]]
[[ $age -gt 18 && $name != "" ]]
[[ $email =~ @.*\. ]]   # Regex match
```

---

## 11.6 Loops

```bash
#!/bin/bash

# for loop — iterate over list:
for fruit in apple banana cherry; do
    echo "Fruit: $fruit"
done

# for loop — range:
for i in {1..5}; do
    echo "Number: $i"
done

# for loop — with step:
for i in {0..20..5}; do    # 0, 5, 10, 15, 20
    echo $i
done

# C-style for loop:
for ((i=0; i<5; i++)); do
    echo "i = $i"
done

# for loop — over files:
for file in *.txt; do
    echo "Processing: $file"
    wc -l "$file"
done

# while loop:
count=1
while [ $count -le 5 ]; do
    echo "Count: $count"
    ((count++))
done

# while loop — reading file line by line:
while IFS= read -r line; do
    echo "Line: $line"
done < input.txt

# until loop (runs UNTIL condition is true):
count=1
until [ $count -gt 5 ]; do
    echo "Count: $count"
    ((count++))
done

# Loop control:
for i in {1..10}; do
    if [ $i -eq 5 ]; then
        break           # Exit loop
    fi
    if [ $i -eq 3 ]; then
        continue        # Skip to next iteration
    fi
    echo $i
done
```

---

## 11.7 Functions

```bash
#!/bin/bash

# Define function:
greet() {
    echo "Hello, $1!"   # $1 = first argument
}
greet "Akash"           # Call with argument

# Function with return value (only 0-255):
is_even() {
    if [ $(($1 % 2)) -eq 0 ]; then
        return 0    # 0 = success/true in bash
    else
        return 1    # Non-zero = failure/false
    fi
}

if is_even 4; then
    echo "4 is even"
fi

# Return string via echo:
get_date() {
    echo $(date +%Y-%m-%d)
}
today=$(get_date)
echo "Today: $today"

# Function with multiple args:
create_user() {
    local username=$1   # 'local' = variable only exists inside function
    local email=$2
    echo "Creating user: $username ($email)"
}
create_user "akash" "akash@example.com"

# Function with default values:
connect() {
    local host=${1:-"localhost"}
    local port=${2:-8080}
    echo "Connecting to $host:$port"
}
connect                         # Uses defaults
connect "myserver.com" 443      # Custom values
```

---

## 11.8 Arrays

```bash
#!/bin/bash

# Declare array:
fruits=("apple" "banana" "cherry" "date")

# Access elements:
echo ${fruits[0]}           # First element: apple
echo ${fruits[1]}           # Second: banana
echo ${fruits[-1]}          # Last: date
echo ${fruits[@]}           # All elements
echo ${#fruits[@]}          # Array length: 4

# Modify array:
fruits[4]="elderberry"      # Add element at index 4
fruits+=("fig")             # Append element

# Loop over array:
for fruit in "${fruits[@]}"; do
    echo "Fruit: $fruit"
done

# Array slicing:
echo ${fruits[@]:1:2}       # 2 elements starting at index 1

# Associative arrays (like dictionaries):
declare -A user
user["name"]="Akash"
user["age"]=25
user["city"]="Bangalore"

echo ${user["name"]}
echo ${user["age"]}

# List all keys:
echo ${!user[@]}
# List all values:
echo ${user[@]}
```

---

## 11.9 String Operations

```bash
#!/bin/bash
str="Hello, World! Hello Linux"

echo ${#str}                    # Length: 25
echo ${str,,}                   # Lowercase: hello, world!...
echo ${str^^}                   # Uppercase: HELLO, WORLD!...
echo ${str:0:5}                 # Substring: Hello (start=0, len=5)
echo ${str:7:5}                 # Substring: World

# Replace:
echo ${str/Hello/Hi}            # Replace FIRST: Hi, World! Hello Linux
echo ${str//Hello/Hi}           # Replace ALL: Hi, World! Hi Linux

# Remove prefix/suffix:
file="archive.tar.gz"
echo ${file%.gz}                # Remove shortest .gz suffix: archive.tar
echo ${file%%.*}                # Remove longest .* suffix: archive
echo ${file#*.}                 # Remove shortest prefix: tar.gz
echo ${file##*.}                # Remove longest prefix: gz

# Check if contains:
if [[ "$str" == *"World"* ]]; then
    echo "Contains 'World'"
fi

# Split string:
IFS=',' read -ra parts <<< "a,b,c,d"
for part in "${parts[@]}"; do
    echo "$part"
done
```

---

## 11.10 File Operations in Scripts

```bash
#!/bin/bash

# Check if file exists:
if [ -f "/etc/hosts" ]; then
    echo "File exists"
fi

# Check if directory exists:
if [ ! -d "$HOME/backups" ]; then
    mkdir -p "$HOME/backups"
    echo "Created backups directory"
fi

# Read file line by line:
while IFS= read -r line; do
    echo "Processing: $line"
done < /etc/hosts

# Write to file:
echo "Log entry: $(date)" > /tmp/mylog.txt      # Overwrite
echo "Another entry" >> /tmp/mylog.txt           # Append

# Process multiple files:
for file in /var/log/*.log; do
    if [ -s "$file" ]; then   # -s = file is non-empty
        echo "=== $file ==="
        tail -5 "$file"
    fi
done

# Create temp file:
tmpfile=$(mktemp /tmp/myapp.XXXXXX)
echo "Temp file: $tmpfile"
# Use it...
rm -f "$tmpfile"              # Clean up
```

---

## 11.11 Exit Codes & Error Handling

```bash
#!/bin/bash
# Every command returns an exit code:
# 0 = success
# 1-255 = error

# Check exit code:
ls /nonexistent
echo "Exit code: $?"      # Non-zero = error

# Exit with code:
exit 0                    # Success
exit 1                    # Error

# Error handling patterns:

# Pattern 1: set -e (exit on error)
set -e                    # Script exits if any command fails
set -u                    # Exit on undefined variable
set -o pipefail           # Exit on pipe failure
# Usually combined:
set -euo pipefail

# Pattern 2: Check each command:
if ! cp source.txt dest.txt; then
    echo "ERROR: Copy failed!" >&2
    exit 1
fi

# Pattern 3: trap for cleanup:
cleanup() {
    echo "Cleaning up..."
    rm -f /tmp/myapp.*
}
trap cleanup EXIT         # Run cleanup() when script exits

# Pattern 4: || for fallback:
mkdir /newdir || { echo "Failed to create dir"; exit 1; }

# Log errors to stderr:
echo "Something went wrong" >&2
```

---

## 11.12 Practical Script Examples

### System Backup Script
```bash
#!/bin/bash
set -euo pipefail

BACKUP_DIR="$HOME/backups"
DATE=$(date +%Y%m%d_%H%M%S)
BACKUP_FILE="$BACKUP_DIR/backup_$DATE.tar.gz"

mkdir -p "$BACKUP_DIR"
echo "Creating backup: $BACKUP_FILE"
tar -czvf "$BACKUP_FILE" ~/Documents ~/Pictures ~/Desktop
echo "Backup complete! Size: $(du -sh "$BACKUP_FILE" | cut -f1)"
```

### System Update Script
```bash
#!/bin/bash
echo "=== System Update ==="
sudo apt update
sudo apt upgrade -y
sudo apt autoremove -y
sudo apt clean
echo "=== Update Complete ==="
```

### Log Monitor Script
```bash
#!/bin/bash
LOG_FILE="/var/log/syslog"
KEYWORD="${1:-error}"

echo "Monitoring $LOG_FILE for '$KEYWORD'..."
tail -f "$LOG_FILE" | grep --line-buffered -i "$KEYWORD"
```

### Check Services Script
```bash
#!/bin/bash
services=("nginx" "ssh" "ufw")

for svc in "${services[@]}"; do
    if systemctl is-active --quiet "$svc"; then
        echo "✅ $svc is running"
    else
        echo "❌ $svc is NOT running"
    fi
done
```

---

## ✅ Module 11 Quick Reference

| Concept | Syntax |
|---------|--------|
| Variable | `name="value"` |
| Read input | `read -p "Prompt: " var` |
| If statement | `if [ condition ]; then ... fi` |
| For loop | `for i in {1..5}; do ... done` |
| While loop | `while [ cond ]; do ... done` |
| Function | `fname() { commands; }` |
| Array | `arr=("a" "b" "c"); echo ${arr[0]}` |
| Exit code | `echo $?` |
| Set strict mode | `set -euo pipefail` |
| Stderr | `echo "Error" >&2` |
| Trap | `trap cleanup EXIT` |
| Arithmetic | `echo $((5 + 3))` |
| String length | `echo ${#var}` |
| Substring | `echo ${var:0:5}` |

---

> **▶ Next Module: [Module 12 — Advanced Commands →](./12-advanced.md)**
