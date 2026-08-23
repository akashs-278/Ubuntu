# Module 10 — Shells in Ubuntu

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 09](./09-process-system.md)** | **[Module 11 →](./11-shell-scripting.md)**

---

## 📋 Table of Contents

- [10.1 What is a Shell?](#101-what-is-a-shell)
- [10.2 Available Shells in Ubuntu](#102-available-shells-in-ubuntu)
- [10.3 Bash — The Default Shell](#103-bash--the-default-shell)
- [10.4 Bash Configuration Files](#104-bash-configuration-files)
- [10.5 Environment Variables](#105-environment-variables)
- [10.6 Bash History & Shortcuts](#106-bash-history--shortcuts)
- [10.7 Command Aliases](#107-command-aliases)
- [10.8 Shell Functions](#108-shell-functions)
- [10.9 Zsh — Z Shell](#109-zsh--z-shell)
- [10.10 Fish Shell](#1010-fish-shell)
- [10.11 Changing Your Default Shell](#1011-changing-your-default-shell)

---

## 10.1 What is a Shell?

A **shell** is a command-line interpreter — the program that reads your commands and executes them.

```
You type command
       │
       ▼
   Shell (bash)          ← Interprets your command
       │
       ▼
  Linux Kernel           ← Executes it
       │
       ▼
  Hardware / OS          ← Does the work
       │
       ▼
  Output back to you
```

**Two types of shells:**
- **Interactive shell** — You type commands manually (your terminal)
- **Non-interactive shell** — Runs shell scripts automatically

---

## 10.2 Available Shells in Ubuntu

```bash
# List all installed shells:
cat /etc/shells

# Common shells:
# /bin/sh     → Bourne Shell (POSIX standard, minimal)
# /bin/bash   → Bash (default in Ubuntu — most common)
# /bin/dash   → Dash (used for system scripts /bin/sh)
# /usr/bin/zsh → Zsh (powerful, popular with devs)
# /usr/bin/fish → Fish (user-friendly, modern)
# /usr/bin/ksh  → KornShell
# /usr/bin/tcsh → TENEX C Shell

# Check your current shell:
echo $SHELL                     # Your default shell
echo $0                         # Current running shell

# Check shell version:
bash --version
zsh --version
fish --version

# Temporarily switch to another shell:
bash        # Start bash session
zsh         # Start zsh session
fish        # Start fish session
exit        # Return to previous shell
```

---

## 10.3 Bash — The Default Shell

Bash (**B**ourne **A**gain **SH**ell) is the default shell in Ubuntu.

### Interactive Features
```bash
# Command history:
history                         # Show all previous commands
history | tail -20              # Last 20 commands
!42                             # Re-run command number 42
!!                              # Re-run the LAST command
sudo !!                         # Re-run last command with sudo
!git                            # Re-run last command starting with "git"
!?nginx?                        # Re-run last command containing "nginx"
history -c                      # Clear history

# Directory navigation shortcuts:
cd -                            # Go to previous directory
pushd /var/log                  # Push to directory stack, go to /var/log
popd                            # Pop — go back to previous directory
dirs                            # Show directory stack

# Tab completion:
ls /ho[TAB]                     # Auto-completes to /home
git che[TAB]                    # Auto-completes to git checkout/cherry-pick
```

### Bash Expansions
```bash
# Brace expansion:
echo {a,b,c}                    # → a b c
mkdir project_{frontend,backend,docs}
touch file_{1..5}.txt           # file_1.txt through file_5.txt
echo {A..Z}                     # → A B C ... Z

# Command substitution:
echo "Today is $(date)"
echo "Files: $(ls | wc -l)"
FILES=$(ls *.txt)
echo $FILES

# Arithmetic:
echo $((5 + 3))                 # → 8
echo $((10 * 5 - 2))            # → 48
result=$((100 / 4))
echo $result                    # → 25

# Parameter expansion:
name="akash"
echo ${name}                    # → akash
echo ${name^^}                  # → AKASH (uppercase)
echo ${name:0:3}                # → aka (substring, 3 chars from pos 0)
echo ${#name}                   # → 5 (string length)
echo ${name:-"default"}         # Use "default" if name is empty
```

---

## 10.4 Bash Configuration Files

Understanding when each file is loaded is crucial:

```
Login shell:     .bash_profile → .bashrc
Non-login shell: .bashrc
System-wide:     /etc/bash.bashrc and /etc/profile
```

### Key Config Files
```bash
~/.bashrc               # Main config — loaded for every interactive shell
~/.bash_profile         # Loaded at login (sources ~/.bashrc)
~/.bash_login           # Alternative to .bash_profile
~/.profile              # Loaded if no .bash_profile exists
~/.bash_logout          # Runs when you log out
/etc/bash.bashrc        # System-wide bashrc (all users)
/etc/profile            # System-wide profile (all users)
/etc/profile.d/         # Directory of scripts sourced by /etc/profile
```

### Editing and Reloading
```bash
nano ~/.bashrc                  # Edit bashrc
source ~/.bashrc                # Reload WITHOUT restarting terminal
. ~/.bashrc                     # Same as source

# Example ~/.bashrc additions:
# Add custom PATH:
export PATH="$PATH:/home/akash/bin"

# Add alias:
alias ll='ls -la'

# Add function:
mkcd() { mkdir -p "$1" && cd "$1"; }
```

---

## 10.5 Environment Variables

Variables that configure the shell and system behavior.

```bash
# View variables:
env                             # All environment variables
printenv                        # Same as env
printenv HOME                   # Specific variable
echo $HOME                      # Print value
echo $PATH                      # Show executable search path
echo $USER                      # Current username
echo $SHELL                     # Current shell
echo $TERM                      # Terminal type
echo $LANG                      # Language/locale setting
echo $EDITOR                    # Default text editor
echo $PS1                       # Bash prompt string

# Set a variable (current session only):
MY_VAR="Hello World"
echo $MY_VAR

# Export to make available to child processes:
export MY_VAR="Hello World"
export DATABASE_URL="postgresql://localhost/mydb"

# Make variable permanent (add to ~/.bashrc):
echo 'export MY_VAR="Hello World"' >> ~/.bashrc
source ~/.bashrc

# Unset a variable:
unset MY_VAR

# Important variables to know:
echo $PATH                      # Directories searched for commands
echo $HOME                      # Your home directory
echo $USER                      # Your username
echo $UID                       # Your user ID
echo $HOSTNAME                  # Computer name
echo $PWD                       # Current directory (like pwd command)
echo $OLDPWD                    # Previous directory
echo $?                         # Exit code of last command (0=success)
echo $!                         # PID of last background command
echo $$                         # Current shell's PID
echo $#                         # Number of script arguments
echo $@                         # All script arguments

# Add directory to PATH:
export PATH="$PATH:/usr/local/myapp/bin"

# Make permanent:
echo 'export PATH="$PATH:/usr/local/myapp/bin"' >> ~/.bashrc
```

---

## 10.6 Bash History & Shortcuts

### Keyboard Shortcuts (readline)
```
Navigation:
  Ctrl+A     → Move cursor to start of line
  Ctrl+E     → Move cursor to end of line
  Ctrl+B     → Move cursor left (backward)
  Ctrl+F     → Move cursor right (forward)
  Alt+B      → Move word left
  Alt+F      → Move word right

Editing:
  Ctrl+U     → Delete from cursor to START of line
  Ctrl+K     → Delete from cursor to END of line
  Ctrl+W     → Delete word before cursor
  Alt+D      → Delete word after cursor
  Ctrl+Y     → Paste (yank) last deleted text
  Ctrl+_     → Undo last edit

History:
  Ctrl+P     → Previous command (up arrow)
  Ctrl+N     → Next command (down arrow)
  Ctrl+R     → Reverse search history (type to search)
  Ctrl+G     → Cancel history search
  Ctrl+S     → Forward search history

Processes:
  Ctrl+C     → Interrupt (kill) current command
  Ctrl+Z     → Suspend current command
  Ctrl+D     → End of input / logout
  Ctrl+L     → Clear screen (like 'clear' command)
```

### History Configuration
```bash
# View history config:
echo $HISTSIZE              # Number of commands kept in memory
echo $HISTFILESIZE          # Number of commands saved to file
echo $HISTFILE              # History file location (~/.bash_history)

# Add to ~/.bashrc for better history:
HISTSIZE=10000
HISTFILESIZE=20000
HISTTIMEFORMAT="%F %T "     # Add timestamps to history
HISTCONTROL=ignoredups:ignorespace   # No duplicate/space entries

# Search history:
history | grep "apt install"    # Find past installs
Ctrl+R → type "nginx"           # Interactive reverse search
```

---

## 10.7 Command Aliases

Aliases are shortcuts for longer commands.

```bash
# Create temporary alias (current session):
alias ll='ls -la'
alias la='ls -A'
alias l='ls -CF'
alias ..='cd ..'
alias ...='cd ../..'
alias grep='grep --color=auto'
alias please='sudo'

# View all aliases:
alias                           # List all active aliases
alias ll                        # Show specific alias definition

# Remove alias:
unalias ll

# Make aliases permanent (add to ~/.bashrc):
nano ~/.bashrc
# Add lines like:
# alias ll='ls -la --color=auto'
# alias update='sudo apt update && sudo apt upgrade -y'
# alias myip='curl ifconfig.me'
source ~/.bashrc                # Apply changes

# Useful alias examples:
alias gs='git status'
alias ga='git add'
alias gc='git commit -m'
alias gp='git push'
alias ports='sudo ss -tulnp'
alias syslog='sudo tail -f /var/log/syslog'
alias update='sudo apt update && sudo apt upgrade -y'
alias df='df -h'
alias du='du -h'
alias free='free -h'
alias mkdir='mkdir -pv'         # Always create parents, verbose
```

---

## 10.8 Shell Functions

Functions are reusable command groups — more powerful than aliases.

```bash
# Define a function:
mkcd() {
    mkdir -p "$1" && cd "$1"
}
mkcd ~/Projects/newproject      # Creates dir and enters it

# Extract any archive:
extract() {
    case "$1" in
        *.tar.gz|*.tgz) tar -xzvf "$1" ;;
        *.tar.bz2)      tar -xjvf "$1" ;;
        *.tar.xz)       tar -xJvf "$1" ;;
        *.zip)          unzip "$1" ;;
        *.gz)           gunzip "$1" ;;
        *.rar)          unrar x "$1" ;;
        *)              echo "Unknown format: $1" ;;
    esac
}
extract archive.tar.gz

# Backup a file:
bak() {
    cp "$1" "$1.bak.$(date +%Y%m%d_%H%M%S)"
    echo "Backup created: $1.bak.*"
}
bak config.conf

# Add to ~/.bashrc to make permanent:
nano ~/.bashrc
source ~/.bashrc
```

---

## 10.9 Zsh — Z Shell

Zsh is a powerful shell popular with developers.

### Installation
```bash
sudo apt install zsh            # Install zsh

# Install Oh My Zsh (framework for zsh):
sh -c "$(curl -fsSL https://raw.github.com/ohmyzsh/ohmyzsh/master/tools/install.sh)"

# Set as default:
chsh -s $(which zsh)            # Change default shell to zsh
# Log out and back in for changes to take effect
```

### Zsh Configuration (`~/.zshrc`)
```bash
nano ~/.zshrc

# Key Oh My Zsh settings:
# ZSH_THEME="agnoster"         # Set theme (many available)
# plugins=(git docker kubectl) # Enable plugins
# source $ZSH/oh-my-zsh.sh     # Load Oh My Zsh

# Reload:
source ~/.zshrc
```

### Zsh Advantages over Bash
```bash
# Better tab completion:
cd /ho[TAB]                     # Completes with menu selection
git [TAB]                       # Shows all git subcommands

# Spelling correction:
cd Docuemnts                    # Zsh asks: "Did you mean Documents?"

# Shared history across sessions

# Glob patterns:
ls **/*.txt                     # Recursive glob (find all .txt)
ls *(.)                         # Only regular files
ls *(/)                         # Only directories
```

---

## 10.10 Fish Shell

Fish is a **friendly interactive shell** — great for beginners.

```bash
# Install fish:
sudo apt install fish

# Start fish:
fish

# Fish advantages:
# - Syntax highlighting as you type
# - Autosuggestions from history (press → to accept)
# - Tab completion works out of the box
# - No need to learn complex config

# Fish config file: ~/.config/fish/config.fish
mkdir -p ~/.config/fish
nano ~/.config/fish/config.fish

# Set variables in fish:
set -x MY_VAR "hello"           # Set environment variable
set -Ux MY_VAR "hello"          # Universal (persists across sessions)

# Aliases in fish (called abbreviations):
abbr --add ll 'ls -la'
abbr --add gs 'git status'

# Fish web config (GUI in browser):
fish_config                     # Opens web-based configuration!
```

---

## 10.11 Changing Your Default Shell

```bash
# View current default shell:
echo $SHELL
grep $USER /etc/passwd | cut -d: -f7

# Change default shell:
chsh -s /bin/bash               # Switch to bash
chsh -s /usr/bin/zsh            # Switch to zsh
chsh -s /usr/bin/fish           # Switch to fish

# List valid shells:
cat /etc/shells

# Apply: Log out and log back in

# Temporarily use a different shell (session only):
bash                            # Bash
zsh                             # Zsh
fish                            # Fish
exit                            # Return to previous shell
```

---

## ✅ Module 10 Quick Reference

| Topic | Key Points |
|-------|-----------|
| **Bash** | Default shell, highly compatible |
| **Zsh** | Better completion, themes, plugins (Oh My Zsh) |
| **Fish** | Most user-friendly, best autosuggestions |
| **~/.bashrc** | Per-user bash config |
| `export VAR=val` | Set environment variable |
| `alias ll='ls -la'` | Create command shortcut |
| `source ~/.bashrc` | Reload config without restart |
| `Ctrl+R` | Search command history |
| `history` | View all previous commands |
| `chsh -s /bin/zsh` | Change default shell |
| `echo $SHELL` | Check current shell |

---

> **▶ Next Module: [Module 11 — Shell Scripting →](./11-shell-scripting.md)**
