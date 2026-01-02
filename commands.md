## Basic Commands

>> Terminal  
>> ```terminal
>> pwd
>> ``` 
>> Shows present working directory.
>
>
>> ```terminal
>> ls
>> ```
>> List directories and contents.
>
>
>> ```terminal
>> cd <name of directory>
>> ``` 
>> Change directory.
>>> Example
>>> ```terminal
>>> cd Documents
>>> ```
>>> It points to the document folder.
>
>
>



## create or delete files/directory
>> ```terminal
>> mkdir <name of directory>
>> ``` 
>> Make/Create directory.
>>> Example
>>> ```terminal
>>> mkdir Drawing
>>> ```
>>> It creates the folder "Drawing".
>
>
>> ```terminal
>> rmdir <name of directory>
>> ``` 
>> Remove empty directory.
>>> Example
>>> ```terminal
>>> rmdir Drawing
>>> ```
>>> It removes the folder "Drawing".
>
>
>> ```terminal
>> rm <name of file/directory>
>> ``` 
>> Delete files/directories
>>> Example
>>> ```terminal
>>> rm Document1
>>> ```
>>> Removes/Deletes the file "Document1"
>
>
>> ```terminal
>> cp <name of file/directory>
>> ``` 
>> Copy files/directories
>>> Example
>>> ```terminal
>>> cp file.txt
>>> ```
>>> Copies "file.txt" file
>
>
>> ```terminal
>> mv <name of file/directory>
>> ``` 
>> move/rename files
>>> Example
>>> ```terminal
>>> mv file.txt Documents/Office
>>> ```
>>> It moves "file.txt" to Office directory
>
>
>> ```terminal
>> touch <file name>
>> ``` 
>> Create an empty file
>>> Example
>>> ```terminal
>>> touch file.txt
>>> ```
>>> It creates a text file named "file.txt"


## Display Contents of a file
>> ```terminal
>> cat <file name>
>> ``` 
>> Display file content
>>> Example
>>> ```terminal
>>> cat file.txt
>>> ```
>>> It displays the content saved inside the "file.txt" file
>
>
>> ```terminal
>> less <file name>
>> ``` 
>> view file content page by page
>>> Example
>>> ```terminal
>>> less file2.txt
>>> ```
>>> It displays the contents of file2.txt (page by page)
>
>
>> ```terminal
>> more <file name>
>> ``` 
>> view file content  
>> It displays the content of a large file with a scroll option
>
>
>> ```terminal
>> head <file name>
>> ``` 
>> shows first line of a file
>
>
>
>> ```terminal
>> tail
>> ``` 
>> show last line of a file




>> ```terminal
>> echo
>> ``` 
>> print message or variables
>
>
>> ```terminal
>> date 
>> ``` 
>> show system date/time
>
>
>> ```terminal
>> whoami
>> ``` 
>> show current user



## Package Management

>>```
>>sudo apt update
>>```
>>update repository index
>
>
>>```
>>sudo apt upgrade
>>```
>>upgrade installed packages
>
>
>>```
>>sudo apt install <package>
>>```
>>install a package
>
>
>>```
>>sudo apt remove <package>
>>```
>>uninstall a package
>
>
>>```
>>sudo apt purge <package>
>>```
>>remove + config files
>
>
>>```
>>sudo apt autoremove
>>```
>>remove unused dependencies
>
>
>>```
>>sudo apt search <term>
>>```
>>search packages
>
>
>>```
>>apt-cache show <package>
>>```
>>show package details
>


##  USER & PERMISSIONS

>>```
>>sudo
>>```
>run as superuser
>
>
>>```
>>chmod
>>```
>change file permissions
>
>
>>```
>>chown
>>```
>change file owner
>
>
>>```
>>chgrp
>>```
>change group owner
>
>
>>```
>>passwd
>>```
>change user password
>
>
>>```
>>su <user>
>>``` 
>switch to another user
>
>
>>```
>>useradd <user>
>>``` 
>create user
>
>
>>```
>>userdel <user>
>>``` 
>delete user
>
>
>>```
>>groupadd <group>
>>``` 
>create group
>
>




## SYSTEM INFORMATION

>>```
>>uname -a
>>```
>kernel/system info
>
>
>>```
>>hostname
>>```
>show system hostname
>
>
>>```
>>df -h
>>```
>disk usage (human readable)
>
>
>>```
>>free -h
>>```
>memory usage
>
>
>>```
>>top
>>```
>real-time process viewer
>
>
>>```
>>htop
>>```
>interactive process viewer
>
>
>>```
>>uptime
>>```
>show uptime
>
>
>>```
>>neofetch
>>```
>display system info (if installed)
>
>



## PROCESS MANAGEMENT

>>```
>>ps
>>```
>>list processes
>
>
>>```
>>ps aux
>>```
>>detailed process listing
>
>
>>```
>>kill <PID>
>>```
>>kill process by ID
>
>
>>```
>>killall <name>
>>```
>>kill processes by name
>
>
>>```
>>jobs
>>```
>>show background jobs
>
>
>>```
>>bg
>>```
>>run job in background
>
>
>>```
>>fg
>>```
>>bring job to foreground
>
>



## FILE & TEXT PROCESSING

>>```
>>grep
>>```
>>search text
>
>
>>```
>>find
>>```
>>search files/directories
>
>
>>```
>>locate
>>```
>>fast file search
>
>
>>```
>>awk
>>```
>>pattern scanning and processing
>
>
>>```
>>sed
>>```
>>stream editing
>
>
>>```
>>cut
>>```
>>cut sections of lines
>
>
>>```
>>sort
>>```
>>sort text
>
>
>>```
>>uniq
>>```
>>show unique lines
>
>
>>```
>>wc
>>```
>>word/line/char count
>
>



## NETWORKING

>>```
>>ping <host>
>>```
>>check connectivity
>
>   
>>```
>>curl <url>
>>```
>>fetch a URL
>
>   
>>```
>>wget <url>
>>```
>>download files
>
>   
>>```
>>ifconfig
>>```
>>network info (net-tools)
>
>   
>>```
>>ip a
>>```
>>show IP addresses
>
>   
>>```
>>ip r
>>```
>>routing table
>
>   
>>```
>>netstat
>>```
>>network statistics
>
>   
>>```
>>ss
>>```
>>socket statistics
>
>   
>>```
>>scp
>>```
>>secure copy files over SSH
>
>   
>>```
>>ssh <user@host>
>>```  
>>remote login via SSH
>
>



## ARCHIVES & COMPRESSION

>>```
>>tar -cvf file.tar dir/
>>```
>>create tar archive
>
>
>>```
>>tar -xvf file.tar
>>```
>>extract tar archive
>
>
>>```
>>tar -czvf file.tar.gz dir/
>>```
>>create tar.gz archive
>
>
>>```
>>tar -xzvf file.tar.gz
>>```
>>extract tar.gz
>
>
>>```
>>zip file.zip dir/
>>```
>>create zip file
>
>
>>```
>>unzip file.zip
>>```
>>extract zip file
>
>




## DISK & STORAGE

>>```
>>mount
>>```
>>mount filesystem
>>```
>>umount
>>```
>>unmount filesystem
>>```
>>fdisk -l            
>>```
>>list disks/partitions
>>```
>>lsblk
>>```
>>show block devices
>>```
>>blkid
>>```
>>show partition UUIDs
>>```
>>du -sh <dir>
>>```
>>folder size


## SERVICES (SYSTEMD)

>>```
>>systemctl status <service>
>>```
>>service status
>
>
>>```
>>systemctl start <service>
>>```
>>start service
>
>
>>```
>>systemctl stop <service>
>>```
>>stop service
>
>
>>```
>>systemctl restart <service>
>>```
>>restart service
>
>
>>```
>>systemctl enable <service>
>>```
>>enable at boot
>
>
>>```
>>systemctl disable <service>
>>```
>>disable at boot
>
>
>>```
>>journalctl -u <service>
>>```
>>logs for service
>
>


## ADVANCED SYSTEM / ADMIN

>>```
>>crontab -e
>>```
>>edit cron jobs
>
>
>>```
>>crontab -l
>>```
>>list cron jobs
>
>
>>```
>>alias
>>```      
>>create command shortcuts
>
>
>>```
>>export VAR=value
>>```
>>set environment variables
>
>
>>```
>>env
>>```
>>list environment variables
>
>

>>```
>>iptables
>>```
>>firewall rules
>
>
>>```
>>ufw enable
>>```
>>enable firewall
>
>
>>```
>>ufw allow <port>
>>```
>>allow port
>
>

>>```
>>dmesg
>>```
>>kernel logs
>
>
>>```
>>strace <cmd>
>>```
>>trace system calls
>
>
>>```
>>lsof
>>```
>>list open files
>
>
>>```
>>vmstat
>>```
>>virtual memory stats
>
>
>>```
>>iostat
>>```
>>I/O statistics
>
>



## DOCKER (ADVANCED)

>>```
>>docker ps
>>```
>>list containers
>
>
>>```
>>docker images
>>```
>>list images
>
>
>>```
>>docker run <img>
>>```
>>run container
>
>
>>```
>>docker stop <id>
>>```
>>stop container
>
>
>>```
>>docker build .
>>```
>>build image
>
>


## SNAP (PACKAGE MANAGER)

>>```
>>snap list
>>```
>>list snap packages
>
>
>>```
>>snap install <pkg>
>>```
>>install snap package
>
>
>>```
>>snap remove <pkg>
>>```
>>remove snap package
>
>

### <p align="center">Made with ❤️ and ☕ in Mysuru<p>