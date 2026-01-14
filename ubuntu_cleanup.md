# Ubuntu Cleanup Commands (Single File)

## System Temporary Files
> #### bash
> ---
> ```bash
> sudo rm -rf /tmp/*
> sudo rm -rf /var/tmp/*
> ```
## APT Cache & Packages
> #### bash
> ---
> ```bash
> sudo apt clean
> sudo apt autoclean
> sudo apt autoremove
> sudo apt clean && sudo apt autoclean && sudo apt autoremove
> ```

## systemd Journal Logs
> #### bash
> ---
> ```bash
> journalctl --disk-usage
> sudo journalctl --vacuum-time=7d
> sudo journalctl --vacuum-size=100M
> ```

## Log Files
> #### bash
> ---
> ```bash
> sudo rm -f /var/log/*.gz
> sudo rm -f /var/log/*.[0-9]
> sudo find /var/log -type f -exec truncate -s 0 {} \;
> ```

## User Cache & Thumbnails
> #### bash
> ---
> ```bash
> rm -rf ~/.cache/*
> rm -rf ~/.cache/thumbnails/*
> ```

## Snap Cache & Old Revisions
> #### bash
> ---
> ```bash
> sudo rm -rf /var/lib/snapd/cache/*
> sudo snap list --all | awk '/disabled/{print $1, $3}' | while read snapname revision; do sudo snap remove "$snapname" --revision="$revision"; done
> ```

## Trash
> #### bash
> ---
> ```bash
> rm -rf ~/.local/share/Trash/*
> ```

## Crash Reports
> #### bash
> ---
> ```bash
> sudo rm -rf /var/crash/*
> ```

## Orphaned Packages
> #### bash
> ---
> ```bash
> sudo apt install deborphan
> sudo apt remove $(deborphan)
> ```

## DNS Cache
> #### bash
> ---
> ```bash
> sudo systemd-resolve --flush-caches
> systemd-resolve --statistics
> ```

## Swap Memory
> #### bash
> ---
> ```bash
> sudo swapoff -a
> sudo swapon -a
> ```
## Flatpak Cleanup
> #### bash
> ---
> ```bash
> flatpak uninstall --unused
> ```

## Old Kernels
> #### bash
> ---
> ```bash
> sudo apt autoremove --purge
> ```

## Aggressive One-Line Cleanup
> #### bash
> ---
> ```bash
> sudo apt clean && sudo apt autoclean && sudo apt autoremove && sudo journalctl --vacuum-time=7d && sudo rm -rf /tmp/* /var/tmp/* /var/crash/* && rm -rf ~/.cache/* ~/.local/share/Trash/*
> ```

## Disk Usage Check
> #### bash
> ---
> ```bash
> df -h
> ```
