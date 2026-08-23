# Module 08 — Networking Commands

> **🏠 [← Back to Main Course](../README.md)** | **[← Module 07](./07-software-management.md)** | **[Module 09 →](./09-process-system.md)**

---

## 📋 Table of Contents

- [8.1 Viewing Network Interfaces](#81-viewing-network-interfaces)
- [8.2 IP Address Management](#82-ip-address-management)
- [8.3 Testing Connectivity](#83-testing-connectivity)
- [8.4 DNS & Name Resolution](#84-dns--name-resolution)
- [8.5 Ports & Connections](#85-ports--connections)
- [8.6 Downloading Files](#86-downloading-files)
- [8.7 SSH — Secure Shell](#87-ssh--secure-shell)
- [8.8 Firewall (UFW)](#88-firewall-ufw)
- [8.9 Network Configuration (Netplan)](#89-network-configuration-netplan)
- [8.10 Advanced Tools](#810-advanced-tools)

---

## 8.1 Viewing Network Interfaces

```bash
ip addr                         # Show all interfaces and IP addresses
ip addr show eth0               # Show specific interface
ip link                         # Show interface status (up/down)
ip -4 addr                      # IPv4 only
ip -6 addr                      # IPv6 only
ip -s link                      # Show stats (packets sent/received)

# Legacy (install net-tools):
sudo apt install net-tools
ifconfig                        # Show all interfaces
```

---

## 8.2 IP Address Management

```bash
ip route                        # Show routing table (includes gateway)
ip route show default           # Default gateway

# Add/remove IP:
sudo ip addr add 192.168.1.100/24 dev eth0
sudo ip addr del 192.168.1.100/24 dev eth0

# Enable/disable interface:
sudo ip link set eth0 up
sudo ip link set eth0 down

# Add static route:
sudo ip route add 10.0.0.0/8 via 192.168.1.1
sudo ip route add default via 192.168.1.1    # Default gateway
```

---

## 8.3 Testing Connectivity

```bash
# ping — Test reachability:
ping google.com                 # Ping continuously (Ctrl+C to stop)
ping -c 4 google.com            # Send 4 packets
ping -c 4 8.8.8.8               # Ping Google DNS
ping localhost                  # Test loopback (should always work)

# traceroute — Trace path to destination:
sudo apt install traceroute
traceroute google.com           # Show each hop
traceroute -n google.com        # Skip hostname lookups (faster)
tracepath google.com            # No root required

# mtr — Interactive live traceroute:
sudo apt install mtr
mtr google.com
```

---

## 8.4 DNS & Name Resolution

```bash
# nslookup:
nslookup google.com             # Resolve domain to IP
nslookup 8.8.8.8                # Reverse lookup (IP → hostname)

# dig (install dnsutils):
sudo apt install dnsutils
dig google.com                  # Full DNS info
dig google.com A                # IPv4 records
dig google.com MX               # Mail server records
dig +short google.com           # Just the IP
dig @8.8.8.8 google.com         # Query using Google's DNS

# host:
host google.com

# Local name files:
cat /etc/resolv.conf            # DNS servers
cat /etc/hosts                  # Local hostname overrides

# Add local entry to /etc/hosts:
echo "127.0.0.1  myapp.local" | sudo tee -a /etc/hosts
```

---

## 8.5 Ports & Connections

```bash
# ss — socket statistics (modern):
ss -tuln                        # Listening TCP+UDP ports
sudo ss -tulnp                  # Same + process names
ss -tn                          # Active TCP connections
ss -s                           # Summary

# Find what uses a port:
sudo ss -tulnp | grep :80       # Port 80

# lsof — list open files/ports:
sudo lsof -i                    # All network connections
sudo lsof -i :80                # Process on port 80
sudo lsof -i tcp                # All TCP connections

# nmap — port scanner:
sudo apt install nmap
nmap localhost                  # Scan localhost
nmap 192.168.1.1                # Scan a host
nmap -p 80,443 google.com       # Specific ports
nmap 192.168.1.0/24             # Scan entire subnet
nmap -sV 192.168.1.1            # Service version detection
```

---

## 8.6 Downloading Files

```bash
# wget:
wget https://example.com/file.zip
wget -O myfile.zip https://example.com/file  # Custom filename
wget -c https://example.com/large.iso        # Resume download
wget -q https://example.com/file             # Quiet mode

# curl:
curl https://example.com                     # Fetch URL
curl -O https://example.com/file.zip         # Download (original name)
curl -o myfile.zip https://example.com/file  # Custom filename
curl -L https://example.com/                 # Follow redirects
curl -I https://example.com                  # Headers only

# API calls with curl:
curl -X GET https://api.example.com/users
curl -X POST -H "Content-Type: application/json" \
     -d '{"name":"Akash"}' https://api.example.com/users
curl -H "Authorization: Bearer TOKEN" https://api.example.com
```

---

## 8.7 SSH — Secure Shell

```bash
# Connect:
ssh username@hostname
ssh akash@192.168.1.100
ssh -p 2222 akash@host          # Custom port
ssh -v akash@host               # Verbose (debug)

# Generate SSH keys (recommended — passwordless login):
ssh-keygen -t ed25519 -C "your@email.com"
# Creates: ~/.ssh/id_ed25519 (private) and ~/.ssh/id_ed25519.pub (public)

# Copy public key to server:
ssh-copy-id akash@192.168.1.100

# SSH config file (~/.ssh/config):
# Host myserver
#     HostName 192.168.1.100
#     User akash
#     IdentityFile ~/.ssh/id_ed25519

ssh myserver   # Connect using alias

# File transfer:
scp file.txt akash@host:/home/akash/         # Upload
scp akash@host:/home/akash/file.txt ./       # Download
scp -r folder/ akash@host:/home/akash/       # Upload directory

# rsync (efficient sync):
rsync -avz folder/ akash@host:/destination/  # Upload/sync
rsync -avz akash@host:/remote/ ./local/      # Download/sync
rsync -avz --delete src/ akash@host:/dest/   # Sync + remove extras
```

---

## 8.8 Firewall (UFW)

```bash
sudo ufw status verbose         # Check status
sudo ufw enable                 # Enable firewall
sudo ufw disable                # Disable
sudo ufw reset                  # Reset all rules

# Allow connections:
sudo ufw allow ssh              # Allow SSH (port 22)
sudo ufw allow 80               # HTTP
sudo ufw allow 443              # HTTPS
sudo ufw allow 8080             # Custom port
sudo ufw allow from 192.168.1.0/24   # Allow subnet

# Deny:
sudo ufw deny 3306              # Block MySQL
sudo ufw deny from 10.0.0.1    # Block IP

# Remove rule:
sudo ufw status numbered        # Show rules with numbers
sudo ufw delete 3               # Delete rule #3
sudo ufw delete allow 80        # Remove allow 80

# Application profiles:
sudo ufw app list
sudo ufw allow 'Nginx Full'     # Allow nginx HTTP+HTTPS
sudo ufw allow 'OpenSSH'

# Default policies (recommended):
sudo ufw default deny incoming
sudo ufw default allow outgoing
```

---

## 8.9 Network Configuration (Netplan)

```bash
ls /etc/netplan/                # View netplan config files
cat /etc/netplan/01-network-manager-all.yaml
sudo netplan apply              # Apply config changes
sudo netplan try                # Apply with 30s rollback
```

**Static IP config (`/etc/netplan/01-netcfg.yaml`):**
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: no
      addresses: [192.168.1.100/24]
      gateway4: 192.168.1.1
      nameservers:
        addresses: [8.8.8.8, 1.1.1.1]
```

**DHCP config:**
```yaml
network:
  version: 2
  renderer: networkd
  ethernets:
    enp3s0:
      dhcp4: yes
```

---

## 8.10 Advanced Tools

```bash
# tcpdump — Capture packets:
sudo tcpdump                            # Capture all traffic
sudo tcpdump -i eth0                    # Specific interface
sudo tcpdump port 80                    # HTTP traffic
sudo tcpdump host 192.168.1.1           # Traffic to/from IP
sudo tcpdump -w capture.pcap            # Save capture

# nmcli — NetworkManager CLI:
nmcli device                            # List devices
nmcli device wifi list                  # Scan WiFi
nmcli device wifi connect "SSID" password "pass"
nmcli connection show                   # Saved connections

# hostname:
hostname                                # Current hostname
hostname -I                             # All IP addresses
sudo hostnamectl set-hostname newname   # Change hostname

# arp — ARP table:
arp -a                                  # IP to MAC mapping

# iperf3 — Network speed test:
sudo apt install iperf3
iperf3 -s                               # Server mode
iperf3 -c server-ip                     # Client mode (test speed)
```

---

## ✅ Module 08 Quick Reference

| Command | Purpose |
|---------|---------|
| `ip addr` | Show IP addresses |
| `ip route` | Show routing table |
| `ping -c 4 host` | Test connectivity |
| `traceroute host` | Trace network path |
| `dig domain` | DNS lookup |
| `sudo ss -tulnp` | Show open ports |
| `wget url` | Download file |
| `curl -O url` | Download file |
| `ssh user@host` | Remote login |
| `ssh-copy-id user@host` | Copy SSH key |
| `scp file user@host:` | Secure file copy |
| `rsync -avz src user@host:` | Sync to remote |
| `sudo ufw allow 22` | Allow port |
| `sudo ufw status` | Firewall status |
| `nmcli device wifi list` | Scan WiFi |

---

> **▶ Next Module: [Module 09 — Process & System Management →](./09-process-system.md)**
