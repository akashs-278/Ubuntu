# Ubuntu Cleanup Commands (Single File)

## System Temporary Files
```bash
sudo rm -rf /tmp/*
sudo rm -rf /var/tmp/*
```
## APT Cache & Packages
sudo apt clean
sudo apt autoclean
sudo apt autoremove
sudo apt clean && sudo apt autoclean && sudo apt autoremove

## systemd Journal Logs
journalctl --disk-usage
sudo journalctl --vacuum-time=7d
sudo journalctl --vacuum-size=100M

## Log Files
sudo rm -f /var/log/*.gz
sudo rm -f /var/log/*.[0-9]
sudo find /var/log -type f -exec truncate -s 0 {} \;

## User Cache & Thumbnails
rm -rf ~/.cache/*
rm -rf ~/.cache/thumbnails/*

## Snap Cache & Old Revisions
sudo rm -rf /var/lib/snapd/cache/*
sudo snap list --all | awk '/disabled/{print $1, $3}' | while read snapname revision; do sudo snap remove "$snapname" --revision="$revision"; done

## Trash
rm -rf ~/.local/share/Trash/*

## Crash Reports
sudo rm -rf /var/crash/*

## Orphaned Packages
sudo apt install deborphan
sudo apt remove $(deborphan)

## DNS Cache
sudo systemd-resolve --flush-caches
systemd-resolve --statistics

## Swap Memory
sudo swapoff -a
sudo swapon -a

## Flatpak Cleanup
flatpak uninstall --unused

## Old Kernels
sudo apt autoremove --purge

## Aggressive One-Line Cleanup
sudo apt clean && sudo apt autoclean && sudo apt autoremove && sudo journalctl --vacuum-time=7d && sudo rm -rf /tmp/* /var/tmp/* /var/crash/* && rm -rf ~/.cache/* ~/.local/share/Trash/*

## Disk Usage Check
df -h

