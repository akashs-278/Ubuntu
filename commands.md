# Ubuntu Basic to Advance Commands Cheat Sheet

## 1. Directory Navigation Commands

### `pwd` :- Prints the current working directory.
> #### bash
> ---
> ```bash
> pwd
> ```

---

### `ls` :- Lists files and directories.
> #### bash
> ---
> ```bash
> ls
> ls -l      # long listing
> ls -a      # show hidden files
> ls -lh     # human-readable sizes
> ```

---

### `cd` :- Changes directory.
> #### bash
> ---
> ```bash
> cd /path/to/directory
> cd ..      # move up one directory
> cd ~       # home directory
> cd -       # previous directory
> ```

---

## 2. File & Directory Management
### `touch` :- Creates an empty file.
> #### bash
> ---
> ```bash
> touch file.txt
> ```

### `mkdir` :- Creates a directory.
> #### bash
> ---
> ```bash
> mkdir folder
> mkdir -p parent/child
> ```

### `rmdir` :- Removes an empty directory.
> #### bash
> ---
> ```bash
> rmdir folder
> 
> ```
### `rm` :- Removes files or directories.
> #### bash
> ---
> ```bash
> rm file.txt
> rm -r folder
> ```
rm -rf folder   # force delete

### `cp` :- Copies files or directories.
> #### bash
> ---
> ```bash
> cp file1 file2
> cp file /path/to/destination
> ```
cp -r folder destination

### `mv` :- Moves or renames files or directories.
> #### bash
> ---
> ```bash
> mv file newname
> mv file /path/to/destination
> ```

## 3. Viewing File Contents
### `cat` :- Displays file contents.
> #### bash
> ---
> ```bash
> cat file.txt
> ```

### `less` :- Views file content page by page.
> #### bash
> ---
> ```bash
> less file.txt
> ```

### `more` :- Displays file contents (basic pagination).
> #### bash
> ---
> ```bash
> more file.txt
> ```

### `head` :- Shows the first lines of a file.
> #### bash
> ---
> ```bash
> head file.txt
> head -n 10 file.txt
> ```

### `tail` :- Shows the last lines of a file.
> #### bash
> ---
> ```bash
> tail file.txt
> tail -n 10 file.txt
> ```

## 4. File Information & Search
### `stat` :- Displays detailed file information.
> #### bash
> ---
> ```bash
> stat file.txt
> ```

### `file` :- Detects file type.
> #### bash
> ---
> ```bash
> file file.txt
> ```

### `find` :- Searches for files and directories.
> #### bash
> ---
> ```bash
> find . -name file.txt
> ```

### `locate` :- Finds files quickly (requires updated database).
> #### bash
> ---
> ```bash
> locate file.txt
> ```

## 5. Permissions & Ownership
### `chmod` :- Changes file permissions.
> #### bash
> ---
> ```bash
> chmod 755 file.sh
> chmod +x script.sh
> ```

### `chown` :- Changes file owner.
> #### bash
> ---
> ```bash
> chown user file.txt
> chown user:group file.txt
> ```

## 6. Disk & File Size
### `du` :- Shows directory size.
> #### bash
> ---
> ```bash
> du -h folder
> ```

### `df` :- Shows disk space usage.
> #### bash
> ---
> ```bash
> df -h
> ```

## 7. Helpful Shortcuts
| Shortcut |	Description |
|----------|----------------|
| .        | Current directory |
| ..       | Parent directory |
| ~        | Home directory |
| *        | Wildcard |
| Ctrl + C | Stop command |
| Tab      | Auto-complete |

## 8. Archive & Compression (Basics)
### `tar` :- Create or extract archives.
> #### bash
> ---
> ```bash
> tar -cvf archive.tar folder
> tar -xvf archive.tar
> ```

### `zip / unzip` :- Compress and extract zip files.
> #### bash
> ---
> ```bash
> zip file.zip file.txt
> unzip file.zip
> ```