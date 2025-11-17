# Linux Command Reference with Comments

## Table of Contents

- [Frequently Used](#frequently-used)
- [Process Management](#process-management)
- [Networking](#networking)
- [File System](#file-system)
- [File Permissions](#file-permissions)
- [Disk Usage](#disk-usage)
- [Search and Filter](#search-and-filter)
- [Package Management](#package-management)
- [System Configuration](#system-configuration)
- [System Monitoring](#system-monitoring)
- [Service Management](#service-management)


## Frequently Used

```bash
# Process monitoring
ps aux | grep python                     # Find python processes
top -p $(pgrep nginx)                    # Monitor specific processes
htop                                     # Interactive process manager

# Network debugging
ss -tulpn | grep :80                     # Check web server port
ping -c 3 database-server                # Test connectivity
curl -I https://api.example.com          # Check api health

# File operations
tail -f /var/log/app.log                 # Follow application logs
find /var/log -name "*.log" -mtime -1    # Today's log files
grep -r "ERROR" /var/log/ | tail -10     # Recent errors

# System health
df -h                                    # Disk space check
free -h                                  # Memory usage
systemctl status docker                  # Service status

# High cpu investigation
top -n 1 -b | grep -A 5 "^top"           # Quick cpu snapshot
ps aux --sort=-%cpu | head -10           # Top cpu users

# Memory issues
ps aux --sort=-%mem | head -10           # Top memory users
cat /proc/meminfo | grep Available       # Available memory

# Disk space emergency
du -sh /* | sort -hr | head -10          # Largest directories
find /var/log -name "*.log" -size +100M  # Large log files
```

## Process Management

```bash
# Process management
ps aux     # Snapshot of all processes
top        # Real-time view (like task manager)
htop       # Enhanced version (install it everywhere)

# Kill processes
ps aux | grep nginx
kill 1234
kill -9 1234
killall nginx

# Process tree
pstree     # Visual process tree
pstree -p  # Include process ids
```

## Networking

```bash
Networking

# Network setup
ip addr show                                         # Show all network interfaces
ip link show                                         # Show network devices
ifconfig                                             # Shows interface configuration

# Port monitoring
netstat -tulpn                                       # Show all listening ports with processes
ss -tulpn                                            # Modern replacement (faster)
lsof -i :80                                          # What's using port 80?
lsof -i :3306                                        # Check if mysql is running

# Troubleshooting
ping google.com                                      # Basic connectivity test
ping -c 4 server.com                                 # Send only 4 packets
traceroute google.com                                # Show every router hop
dig google.com                                       # Detailed dns lookup
nslookup google.com                                  # Simple dns check

# Ubuntu/debian firewall (ufw - user friendly firewall)
sudo ufw status                                      # Check firewall status
sudo ufw enable                                      # Turn on firewall
sudo ufw allow 22                                    # Allow ssh
sudo ufw allow 80/tcp                                # Allow http traffic
sudo ufw deny 8080                                   # Block specific port

# Traditional iptables
sudo iptables -L                                     # List all rules
sudo iptables -A INPUT -p tcp --dport 443 -j ACCEPT  # Allow https
```

## File System

```bash
File System Navigation

# Basic movement
pwd                               # Where am i?
cd /var/log                       # Go to logs directory
cd ..                             # Go up one level
cd ~                              # Go home
cd -                              # Go to previous directory

# Create update delete files
mkdir -p /path/to/deep/directory  # Create nested directories
touch config.yaml                 # Create empty file
cp app.py app.py.backup           # Backup before changes
cp -r /source/dir /destination/   # Copy entire directories
mv oldname.txt newname.txt        # Rename files
rm filename                       # Delete file
rm -rf directory                  # Delete directory and contents (dangerous!)

# Quick file inspection
cat config.yaml                   # Show entire file
head -20 app.log                  # First 20 lines
tail -50 error.log                # Last 50 lines
tail -f application.log           # Follow log in real-time (lifesaver for debugging)

# Paginated viewing
less large-file.txt               # Navigate large files
```

## File Permissions

```bash
Understanding the Permission Matrix
rwx rwx rwx
│   │   └── Others (everyone else)
│   └────── Group (file's group)
└────────── Owner (file creator)
r = read (4)    - Can view file content
w = write (2)   - Can modify file
x = execute (1) - Can run file as program

# Symbolic notation (readable)
chmod u+x script.sh                      # Give owner execute permission
chmod g-w file.txt                       # Remove group write permission
chmod o+r document.pdf                   # Give others read permission
chmod a+x binary                         # Give everyone execute permission

Numeric notation (faster once you learn it)
chmod 755 script.sh                      # Rwxr-xr-x (common for scripts)
chmod 644 config.txt                     # Rw-r--r-- (common for config files)
chmod 600 private.key                    # Rw------- (private keys)
chmod 777 file.txt                       # Rwxrwxrwx (avoid this - security risk!)

# Change file ownership
sudo chown user:group file.txt           # Change both user and group
sudo chown jenkins config.yaml           # Change only owner
sudo chown :docker script.sh             # Change only group

# Recursive ownership changes
sudo chown -R nginx:nginx /var/www/html  # Change entire directory tree
```

## Disk Usage

```bash
# Check available space
df -h                                   # Human-readable disk usage
df -i                                   # Check inode usage (can run out even with space)

du -h /var/log                          # Directory size
du -sh *                                # Size of each item in current directory
du -h --max-depth=1 /                   # Top-level directory sizes

# Find large files
find / -size +100M -type f 2>/dev/null  # Files larger than 100mb
find /var/log -name "*.log" -size +50M  # Large log files

# Find old files
find /tmp -type f -mtime +30            # Files older than 30 days
```

## Search and Filter

```bash
# Find files by name
find /var/log -name "*.log"                           # All log files
find /etc -name "*nginx*"                             # Anything with nginx in name
find / -type f -name "config.yaml"                    # Specific filename anywhere

# Find by size and date
find /var/log -size +100M                             # Large log files
find /tmp -mtime +7                                   # Files older than 7 days
find /etc -type f -perm 777                           # World-writable files (security risk)

# Grep
grep "ERROR" /var/log/app.log                         # Find errors in logs
grep -r "database" /etc/                              # Recursively search for "database"
grep -i "warning" *.log                               # Case-insensitive search
grep -n "failed" app.log                              # Show line numbers
grep -v "DEBUG" app.log                               # Exclude debug messages

# Real-world log analysis
grep "500" /var/log/nginx/access.log | wc -l          # Count 500 errors
grep "$(date '+%Y-%m-%d')" /var/log/app.log           # Today's logs only

# Awk - column extraction magic
awk '{print $1}' /var/log/nginx/access.log            # Extract ip addresses
awk -F: '{print $1}' /etc/passwd                      # Extract usernames
awk '$9 == 404 {print $1}' /var/log/nginx/access.log  # Ips with 404 errors

# Sed - stream editing
sed 's/old/new/g' config.txt                          # Replace all occurrences
sed -n '10,20p' large-file.txt                        # Print lines 10-20

# Cut - simple column extraction
cut -d: -f1 /etc/passwd                               # First field using : delimiter
cut -c1-10 filename                                   # Characters 1-10 of each line

# Sort and unique analysis
sort /var/log/ips.txt | uniq -c | sort -nr            # Count and sort unique ips
```

## Package Management

```bash
Package Management

# Apt (ubuntu/debian) – the most common
# Update package database first (always!)
sudo apt update                     # Refresh package lists
sudo apt upgrade                    # Upgrade installed packages

# Install software
sudo apt install nginx              # Install web server
sudo apt install htop git curl      # Multiple packages at once

# Remove software
sudo apt remove nginx               # Remove package
sudo apt purge nginx                # Remove package and configs
sudo apt autoremove                 # Clean up orphaned dependencies

# Search for packages
apt search docker                   # Find docker-related packages
apt list --installed | grep python  # List installed python packages

# Yum/dnf (redhat/centos/fedora)
# Yum (older rhel/centos systems)
sudo yum update                     # Update all packages
sudo yum install docker             # Install docker
sudo yum remove docker              # Remove docker

# Dnf (newer fedora/rhel systems)
sudo dnf update                     # Update packages
sudo dnf install podman             # Install container runtime
sudo dnf search kubernetes          # Search for packages
```

## System Configuration

```bash
# Know your system
uname -a                        # Complete system info
lscpu                           # Cpu information
free -h                         # Memory usage
lsblk                           # Block devices (disks)
df -h                           # Disk usage

# Os and version info
cat /etc/os-release             # Os version details
hostnamectl                     # Hostname and system info

# Profile files (loaded in order)
/etc/profile                    # System-wide login profile
~/.bash_profile                 # User login profile
~/.bashrc                       # User interactive shell config

# Create useful aliases
alias ll='ls -la'               # Long listing
alias la='ls -la'               # All files with details
alias grep='grep --color=auto'  # Colored grep output
alias k='kubectl'               # Kubernetes shortcut

# Make aliases permanent
echo "alias ll='ls -la'" >> ~/.bashrc
source ~/.bashrc                # Reload configuration

# View environment
env                             # All environment variables
echo $PATH                      # Show path variable
echo $HOME                      # Home directory

# Set temporary variables
export API_KEY="your-key-here"  # For current session

# Set permanent variables
echo 'export API_KEY="your-key-here"' >> ~/.bashrc
source ~/.bashrc                # Reload to activate
```

## System Monitoring

```bash
# System overview
uptime             # Load average and uptime
w                  # Who's logged in and doing what
top                # Real-time process monitoring
htop               # Enhanced process viewer

# Memory analysis
free -h            # Memory usage summary
cat /proc/meminfo  # Detailed memory info
vmstat 1 5         # Virtual memory stats (1 sec intervals, 5 times)

# I/o performance
iostat 1 5         # Disk i/o statistics
sar 1 5            # System activity report

# Check swap status
swapon --show      # Active swap partitions
cat /proc/swaps    # Swap usage details
free -h            # Memory and swap summary
```

## Service Management

```bash
# Basic service operations
sudo systemctl start nginx                               # Start web server
sudo systemctl stop nginx                                # Stop web server
sudo systemctl restart nginx                             # Restart (stop then start)
sudo systemctl reload nginx                              # Reload config without restart

# Service status and health
systemctl status nginx                                   # Detailed service status
systemctl is-active nginx                                # Quick active check
systemctl is-enabled nginx                               # Check if starts at boot

# Enable/disable services
sudo systemctl enable nginx                              # Start automatically at boot
sudo systemctl disable nginx                             # Don't start at boot

# List services
systemctl list-units --type=service                      # All services
systemctl list-units --failed                            # Only failed services
systemctl list-unit-files --type=service | grep enabled  # Boot-enabled services

# Log investigation
journalctl -u nginx                                      # Service-specific logs
journalctl -u nginx --since "1 hour ago"                 # Recent logs
journalctl -f -u nginx                                   # Follow logs in real-time

# Traditional service command (still works)
sudo service nginx start                                 # Start service
sudo service nginx status                                # Check status
sudo /etc/init.d/nginx restart                           # Direct init script
```