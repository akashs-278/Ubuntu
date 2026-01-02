### This file contains all the necessary changes that must made after installing the ```Ubuntu``` a ```Linux``` distribution.


## 1. Update
>```terminal
>sudo apt update
>```

## 2. Upgrade
>```terminal
>sudo apt upgrade
>```

- Update and Upgrade (Updates everything installed via APT)
>Type 1 :- Terminal
>```terminal
>sudo apt update && sudo apt upgrade
>```
Requires password

>Type 2 :- Terminal
>```terminal
>sudo apt update && sudo apt upgrade -y
>``` 
Doesn't requires password

## 3. Install ```build essential```
download and install C, C++, Python packages.
>Terminal
>```
>sudo apt install build essential
>```

## 4. Update kernel + Major changes 
>Terminal
>```
>sudo apt full-upgrade -y
>```

## 5. Remove unused packages
>```
>sudo apt autoremove -y
>```


## 6. Brightness Controller
Step 1 :- Open Terminal and below command
>```
>sudo nano /etc/default/grub
>```

Step 2 :- Find line
>```
>GRUB_CMDLINE_LINUX_DEFAULT="quiet splash" 
>```

Step 3 :- Replace it by
>```
>GRUB_CMDLINE_LINUX_DEFAULT="quiet splash acpi_backlight=vendor"
>```

Step 4 :- Update ```grub``` information
>```
>sudo update-grub
>```

Step 5 :- Restart your system
>```
>sudo reboot
>```

## 7. Extensions
>```
>sudo apt install gnome-shell-extension-prefs
>```

## 8. Extensions Manager
>```
>sudo apt install gnome-shell-extension-manager
>```

## 9. Tweaks
>```
>sudo apt install gnome-tweaks
>```

## 10. Github
How to install github desktop on ubuntu:
> Download .deb file:
>```sudo wget https://github.com/shiftkey/desktop/releases/download/				
>release-3.2.0-linux1/GitHubDesktop-linux-3.2.0-linux1.deb
>```

>gdebi: 
>``` 
>sudo apt install gdebi-core -y
>sudo gdebi GitHubDesktop-linux-3.2.0-linux1.deb
>```

>dpkg:
>```
>sudo dpkg -i GitHubDesktop-linux-3.2.0-linux1.deb
>```

## 11. Open  RGB
Step 1 :- Download ```.deb``` file from
>```
>https://openrgb.org/releases.html
>```

Step 2 :- Install it
>```
>cd ~/Downloads
>sudo dpkg -i openrgb_*amd64.deb
>```

Step 3 :- If missing some dependencies fix it
>```
>sudo apt --fix-broken install
>```

## 12. YT-DLP
Direct download and install from system.
>Terminal
>```terminal
>sudo apt install yt-dlp  
>```

Dowbload and install from python package manager ```pip```
>Python
>```
>pip install yt-dlp
>```

## 13. VLC Media Player
>```
>sudo snap install vlc
>```

## 14. Blender
>```
>sudo apt install blender
>```

## 15. Mission Center
>```
>flatpak install flathub io.missioncenter.MissionCenter
>```

>`Run`  
>```
>flatpak run io.missioncenter.MissionCenter
>```

## 16. Flathub `Software` store
To install Flatpak on Ubuntu 18.10 (Cosmic Cuttlefish) or later, open the Terminal app and run:
>```
>sudo apt install flatpak
>```

The GNOME Software plugin makes it possible to install apps without needing the command line. To install, run:
>```
>sudo apt install gnome-software-plugin-flatpak
>```

Flathub is the best place to get Flatpak apps. To enable it, run:
>Terminal
>---
>```
>flatpak remote-add --if-not-exists flathub https://dl.flathub.org/repo/flathub.flatpakrepo
>```

To complete setup, restart your system. Now all you have to do is install apps!