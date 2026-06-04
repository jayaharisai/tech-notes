Below is a complete, end‑to‑end Linux guide – from absolute beginner to advanced level. It covers daily use, system administration, databases, networking, scripting, performance tuning, and everything you need to crack a senior Linux job.  
You can copy everything below and save it as `linux_complete_guide.md`.

```markdown
# Complete Linux Guide: From Beginner to Senior-Level Expert

## Table of Contents
1. [Introduction to Linux](#1-introduction-to-linux)
2. [Getting Started: First Steps](#2-getting-started-first-steps)
3. [The Linux Filesystem](#3-the-linux-filesystem)
4. [Essential Commands – Daily Use](#4-essential-commands--daily-use)
5. [File Permissions and Ownership](#5-file-permissions-and-ownership)
6. [Working with Text Files](#6-working-with-text-files)
7. [Process Management](#7-process-management)
8. [Package Management](#8-package-management)
9. [User and Group Administration](#9-user-and-group-administration)
10. [Networking Fundamentals](#10-networking-fundamentals)
11. [Security – Firewall, SSH, SELinux/AppArmor](#11-security--firewall-ssh-selinuxapparmor)
12. [Shell Scripting (Bash)](#12-shell-scripting-bash)
13. [System Administration & Automation](#13-system-administration--automation)
14. [Disk Management & LVM](#14-disk-management--lvm)
15. [Performance Monitoring & Tuning](#15-performance-monitoring--tuning)
16. [Log Management and Journalctl](#16-log-management-and-journalctl)
17. [Databases on Linux (MySQL/PostgreSQL)](#17-databases-on-linux-mysqlpostgresql)
18. [Backup and Recovery Strategies](#18-backup-and-recovery-strategies)
19. [Troubleshooting Common Issues](#19-troubleshooting-common-issues)
20. [Advanced Topics – Kernel, Systemd, Containers](#20-advanced-topics--kernel-systemd-containers)
21. [Interview Prep – Senior Linux Role](#21-interview-prep--senior-linux-role)
22. [Cheat Sheets & Quick References](#22-cheat-sheets--quick-references)

---

## 1. Introduction to Linux

**What is Linux?**  
Linux is an open‑source, Unix‑like operating system kernel first released by Linus Torvalds in 1991. Distributions (Ubuntu, RHEL, Debian, CentOS, etc.) package the kernel with GNU tools and other software.

**Why Linux for senior roles?**  
- Powers 90%+ of cloud infrastructure and servers  
- Used in embedded systems, supercomputers, containers  
- Strong command‑line culture – automation and scripting are key

**Common distro families:**  
- **Debian based:** Ubuntu, Linux Mint, Kali (uses `apt`)  
- **Red Hat based:** RHEL, CentOS, Fedora (uses `yum`/`dnf`)  
- **SUSE based:** openSUSE (uses `zypper`)  
- **Others:** Arch (Pacman), Alpine (apk)

---

## 2. Getting Started: First Steps

### Logging in
- **Console / Virtual Terminal:** Ctrl+Alt+F1..F6  
- **GUI Terminal:** Ctrl+Alt+T (most distros)

### Who am I? Where am I?
```bash
whoami          # current username
pwd             # print working directory
hostname        # system name
id              # user ID and group info
```

### Getting help
```bash
man <command>   # manual page (press q to quit)
<command> --help
info <command>
```

### Shutting down / rebooting
```bash
shutdown -h now   # halt immediately
reboot
systemctl poweroff   # using systemd
```

---

## 3. The Linux Filesystem

Linux uses a single rooted tree hierarchy – everything starts from `/` (root).

| Directory | Description |
|-----------|-------------|
| `/`       | Root – top of the filesystem |
| `/bin`    | Essential user binaries (ls, cp, mv) |
| `/sbin`   | System binaries (fdisk, mkfs) |
| `/etc`    | Configuration files (passwd, network) |
| `/home`   | User home directories |
| `/root`   | Home for root user |
| `/var`    | Variable data (logs, spool, temp) |
| `/tmp`    | Temporary files (cleared on reboot) |
| `/usr`    | User programs and libraries |
| `/proc`   | Virtual filesystem – kernel & process info |
| `/dev`    | Device files (hard disks, terminals) |
| `/mnt` / `/media` | Mount points for removable drives |

**Path types:**
- Absolute: start with `/` (e.g., `/home/bob/file`)
- Relative: no leading `/` (e.g., `Documents/file`)

---

## 4. Essential Commands – Daily Use

### File Operations
```bash
ls -la                 # list all files with details
cd /etc                # change directory
cp source dest         # copy file/dir
cp -r source/ dest/    # recursive copy
mv old new             # move or rename
rm file                # remove file
rm -rf dir/            # remove directory recursively (dangerous!)
mkdir newdir           # create directory
rmdir emptydir         # remove empty directory
touch file.txt         # create empty file or update timestamp
```

### Viewing Files
```bash
cat file               # print whole file
less file              # scrollable view (q to quit)
head -20 file          # first 20 lines
tail -f file           # follow new lines (logs!)
```

### Finding Files
```bash
find /home -name "*.txt"        # search by name
find /var -type f -size +10M     # files larger than 10MB
grep -r "text" /etc/             # search inside files
locate myfile                    # fast search from database (updatedb first)
which ls                         # show location of executable
whereis python                   # show binary, source, man
```

### Text manipulation (pipes and redirection)
```bash
|   # pipe – take output of left as input to right
>   # redirect output to file (overwrite)
>>  # redirect append
<   # read input from file
2>  # redirect errors only
&>  # redirect both stdout and stderr

# Examples:
ls -la | grep "Aug"         # show files modified in August
ps aux | grep "nginx"       # check nginx processes
echo "Hello" > file.txt     # write to file
cat error.log 2>/dev/null   # discard errors
```

---

## 5. File Permissions and Ownership

Every file has:  
- **Owner** (user)  
- **Group**  
- **Permissions**: read (r), write (w), execute (x) for each of: owner / group / others

View with `ls -l` → example: `-rwxr-xr--`  
`-` file type (d=directory, l=symlink), then three triplets: `rwx` (owner), `r-x` (group), `r--` (others)

### Changing permissions (chmod)
```bash
chmod 755 script.sh      # owner: rwx, group: r-x, others: r-x
chmod u+x file           # add execute for user only
chmod go-w file          # remove write for group and others
```

**Numeric mode:**  
- r=4, w=2, x=1 → sum for each triplet. Example: 754 = owner(7=r+w+x), group(5=r+x), others(4=r)

### Changing ownership (chown, chgrp)
```bash
chown bob file           # set owner to bob
chown bob:developers file # set owner and group
chgrp admins file        # set group only
```

### Special permissions – SUID, SGID, Sticky bit
- **SUID (4xxx)** – runs with owner’s privileges (e.g., `/bin/passwd`)
- **SGID (2xxx)** – runs with group’s privileges / new files inherit group
- **Sticky (1xxx)** – only file owner can delete (common on `/tmp`)

```bash
chmod 4755 file   # add SUID (4)
chmod 1777 /tmp   # add sticky bit
```

### umask – default permissions for new files
```bash
umask 022         # new files: 644, new dirs: 755
umask            # show current
```

---

## 6. Working with Text Files

### Editors
- **nano** – beginner friendly
- **vim** – advanced, modal editor
- **sed** / **awk** – stream editors for scripting

### grep (search inside files)
```bash
grep "error" logfile
grep -i "warning" logfile     # case insensitive
grep -r "pattern" /etc/       # recursive
grep -v "ignore" file         # invert match
grep -E "regex" file          # extended regex
```

### sed (find and replace)
```bash
sed 's/old/new/g' file.txt        # replace all old with new (prints)
sed -i 's/old/new/g' file.txt     # edit file in place
sed '/pattern/d' file             # delete lines matching pattern
```

### awk (column/field processing)
```bash
awk '{print $1}' file             # first column
awk -F: '{print $1,$6}' /etc/passwd   # : as separator
awk '$3 > 1000 {print $1}' /etc/passwd
```

### Sorting and unique
```bash
sort file
sort -r file          # reverse
sort -n file          # numeric
uniq                   # remove adjacent duplicates (usually with sort)
sort file | uniq -c   # count occurrences
```

### Cut and paste
```bash
cut -d: -f1 /etc/passwd      # first field
paste file1 file2            # merge columns
```

### wc (word count)
```bash
wc -l file        # lines
wc -w file        # words
wc -c file        # bytes
```

---

## 7. Process Management

### Viewing processes
```bash
ps aux                # all processes (BSD style)
ps -ef                # system V style
top                   # live updating processes
htop                  # nicer top (install if missing)
pstree                # process tree
```

### Signals and killing processes
```bash
kill PID              # send SIGTERM (graceful)
kill -9 PID           # SIGKILL (force)
kill -15 PID          # SIGTERM (same as default)
pkill -f name         # kill by command name pattern
killall firefox       # kill all processes named firefox
```

### Foreground / background
```bash
command &             # run in background
Ctrl+Z                # suspend foreground job
jobs                  # list background/suspended jobs
bg %1                 # resume job 1 in background
fg %1                 # bring to foreground
```

### Nice / renice (priority)
Nice values: -20 (highest) to 19 (lowest)
```bash
nice -n 10 long_script.sh &
renice 5 -p 1234
```

### Systemd process management (services)
```bash
systemctl status sshd
systemctl start/stop/restart nginx
systemctl enable docker   # start at boot
systemctl disable docker
systemctl list-units --type=service
```

---

## 8. Package Management

### Debian / Ubuntu (apt)
```bash
sudo apt update                # refresh package index
sudo apt upgrade               # upgrade all packages
sudo apt install nginx         # install a package
sudo apt remove nginx          # remove but keep configs
sudo apt purge nginx           # remove everything
apt search "web server"        # search
apt show nginx                 # show details
sudo apt autoremove            # remove unneeded deps
```

### Red Hat / CentOS (yum / dnf)
```bash
sudo dnf update
sudo dnf install httpd
sudo dnf remove httpd
dnf search nginx
dnf info nginx
```

### Compiling from source (advanced)
```bash
wget https://example.com/soft.tar.gz
tar -xzf soft.tar.gz
cd soft
./configure --prefix=/usr/local
make
sudo make install
```

### Managing repos
```bash
# Debian: /etc/apt/sources.list
sudo add-apt-repository ppa:some/ppa

# RHEL: /etc/yum.repos.d/
```

---

## 9. User and Group Administration

### User management
```bash
sudo useradd -m -G wheel bob     # create user with home, secondary group
sudo passwd bob                  # set password
sudo usermod -aG sudo alice      # add to sudo group
sudo userdel -r bob              # delete user and home
id bob                           # UID, GIDs
who, w                           # who is logged in
```

### Group management
```bash
sudo groupadd developers
sudo groupdel developers
sudo gpasswd -a bob developers   # add user to group
sudo gpasswd -d bob developers   # remove
groups bob                       # show groups for user
```

### Sudoers (/etc/sudoers)
Edit safely with `visudo`. Examples:
```
bob ALL=(ALL) ALL                # bob can do everything
%developers ALL=(ALL) NOPASSWD: /bin/systemctl restart nginx
```

### Switching users
```bash
su - bob           # switch to bob (new environment)
sudo -i            # root shell with root's env
sudo -u bob command   # run command as bob
```

---

## 10. Networking Fundamentals

### Check configuration
```bash
ip addr show              # all IP addresses
ip link set eth0 up       # bring interface up
ping -c 4 google.com      # test connectivity
ss -tulpn                 # listening ports (modern netstat)
netstat -tulpn            # if netstat installed
```

### Routing
```bash
ip route show
route -n
sudo ip route add default via 192.168.1.1
```

### DNS and name resolution
```bash
cat /etc/resolv.conf      # DNS servers
dig google.com            # detailed DNS query
nslookup google.com
host google.com
```

### Downloading files
```bash
wget https://example.com/file.tar.gz
curl -O https://example.com/file
curl -o localname.txt http://site/file
```

### Firewall basics (ufw / firewalld / iptables)
```bash
# ufw (Ubuntu)
sudo ufw allow 22/tcp
sudo ufw enable
sudo ufw status

# firewalld (RHEL)
sudo firewall-cmd --add-port=80/tcp --permanent
sudo firewall-cmd --reload

# iptables (legacy)
sudo iptables -A INPUT -p tcp --dport 22 -j ACCEPT
```

### SSH (Secure Shell)
```bash
ssh user@remote_host
ssh -p 2222 user@host
ssh -i mykey.pem user@host    # key authentication

# Copy files with SCP
scp file.txt user@host:/path/
scp -r dir/ user@host:/path/

# Generate SSH keys
ssh-keygen -t rsa -b 4096
ssh-copy-id user@host         # copy public key to remote
```

### Network troubleshooting
```bash
traceroute google.com          # path to destination
mtr google.com                 # continuous traceroute/ping
tcpdump -i eth0 port 80        # capture packets
nc -zv google.com 443          # check port open (netcat)
```

---

## 11. Security – Firewall, SSH, SELinux/AppArmor

### SSH hardening
- Disable root login: `PermitRootLogin no` in `/etc/ssh/sshd_config`
- Use key auth only: `PasswordAuthentication no`
- Change default port (optional)
- Allow only specific users: `AllowUsers bob alice`

### SELinux (Red Hat family)
```bash
getenforce                # Enforcing / Permissive / Disabled
setenforce 0              # temporarily permissive
sestatus                  # detailed status
ausearch -m avc -ts recent  # check denials
chcon -t httpd_sys_content_t /var/www/html/
semanage fcontext -a -t httpd_sys_content_t "/web(/.*)?"
restorecon -R /web
```

### AppArmor (Ubuntu / Debian)
```bash
aa-status
aa-complain /path/to/profile
aa-enforce /path/to/profile
```

### Security auditing
```bash
auditd                     # install and configure
ausearch -k mykey
```

---

## 12. Shell Scripting (Bash)

### Shebang and basics
```bash
#!/bin/bash
# This is a comment
echo "Hello, $USER"
```

### Variables
```bash
name="John"
echo $name
readonly PI=3.14
export GLOBAL_VAR="available to child processes"
```

### Command substitution
```bash
today=$(date +%Y-%m-%d)
files=`ls`    # old style, use $(...) instead
```

### Input / Output
```bash
read -p "Enter your name: " username
echo "You entered: $username"
```

### Conditionals (if)
```bash
if [ -f "/etc/passwd" ]; then
    echo "passwd exists"
elif [ -d "/etc" ]; then
    echo "/etc is a directory"
else
    echo "none"
fi
```

Test operators:  
- `-f file` → exists and regular file  
- `-d dir` → exists and directory  
- `-z string` → empty string  
- `-n string` → non‑empty  
- `num1 -eq num2` → equal  
- `-lt`, `-gt`, `-le`, `-ge`  

### Loops
```bash
# for loop
for i in {1..5}; do
    echo "Number $i"
done

# while loop
count=1
while [ $count -le 5 ]; do
    echo $count
    ((count++))
done
```

### Case statement
```bash
case $1 in
    start)
        echo "Starting..."
        ;;
    stop)
        echo "Stopping..."
        ;;
    *)
        echo "Usage: $0 {start|stop}"
        ;;
esac
```

### Functions
```bash
myfunc() {
    local local_var="only inside"
    echo "Argument 1: $1"
    return 0
}
myfunc "hello"
echo "Exit status: $?"
```

### Exit codes
- `0` → success  
- `1-255` → error

### Debugging scripts
```bash
bash -x script.sh        # trace execution
set -x                   # inside script to enable trace
set -e                   # exit on any error
```

### Useful one‑liners for scripts
```bash
# Check if script is run as root
if [ "$EUID" -ne 0 ]; then
    echo "Please run as root"
    exit
fi

# Count lines in log from today
grep "$(date +%Y-%m-%d)" /var/log/syslog | wc -l
```

---

## 13. System Administration & Automation

### Cron jobs (scheduled tasks)
```bash
crontab -e                # edit your own crontab
crontab -l                # list
crontab -r                # remove
```

**Crontab syntax:**  
`minute hour day month day-of-week command`  
Example: `30 2 * * * /usr/bin/backup.sh` → 2:30am daily

Special strings:  
`@reboot`, `@daily`, `@hourly`, `@weekly`, `@monthly`

### Systemd timers (modern cron alternative)
Create `/etc/systemd/system/mytimer.timer` and `.service`

### Automating tasks with Ansible (basic)
```bash
ansible all -m ping
ansible-playbook playbook.yml
```

### Environment variables
Important files: `/etc/environment`, `/etc/profile`, `~/.bashrc`, `~/.profile`
```bash
export PATH=$PATH:/custom/bin
echo $PATH
```

### Kernel parameters (sysctl)
```bash
sysctl -a                 # list all
sysctl net.ipv4.ip_forward   # read value
sudo sysctl -w net.ipv4.ip_forward=1   # temporary
# permanent: add line to /etc/sysctl.conf
```

---

## 14. Disk Management & LVM

### View disks and partitions
```bash
lsblk                    # tree view of block devices
fdisk -l /dev/sda        # show partitions
df -h                    # disk usage of mounted filesystems
du -sh /home/            # size of directory
```

### Partitioning (fdisk)
```bash
sudo fdisk /dev/sdb
# Commands: n (new), p (print), w (write), q (quit)
```

### Filesystem creation
```bash
mkfs.ext4 /dev/sdb1      # create ext4 filesystem
mkfs.xfs /dev/sdb1       # XFS
```

### Mounting
```bash
mount /dev/sdb1 /mnt/data
umount /mnt/data
# Permanent mount: edit /etc/fstab
# Example line: /dev/sdb1 /mnt/data ext4 defaults 0 2
```

### LVM (Logical Volume Manager)
Ideal for flexible resizing.
```bash
# Physical Volume
pvcreate /dev/sdb1
# Volume Group
vgcreate vg_data /dev/sdb1
# Logical Volume
lvcreate -L 10G -n lv_home vg_data
# Create filesystem
mkfs.ext4 /dev/vg_data/lv_home
# Mount
mount /dev/vg_data/lv_home /home
```

Extend LV:
```bash
lvextend -L +5G /dev/vg_data/lv_home
resize2fs /dev/vg_data/lv_home   # ext4
xfs_growfs /mountpoint           # XFS
```

### Swap
```bash
swapon -s                  # show swap
fallocate -l 2G /swapfile
chmod 600 /swapfile
mkswap /swapfile
swapon /swapfile
# add to /etc/fstab: /swapfile none swap sw 0 0
```

---

## 15. Performance Monitoring & Tuning

### System load
```bash
uptime                     # load average (1,5,15 minutes)
top / htop
mpstat -P ALL 1            # per‑CPU usage (sysstat package)
```

### Memory
```bash
free -h                    # RAM + swap
vmstat 2 5                 # processes, memory, swap, IO
smem                       # PSS memory (accurate)
```

### I/O and disk performance
```bash
iostat -x 1                # disk I/O stats
iotop                      # per‑process I/O (requires root)
df -i                      # inode usage
```

### Network performance
```bash
sar -n DEV 1               # network stats (sysstat)
iftop, nethogs             # bandwidth per connection
```

### Find resource hogs
```bash
ps aux --sort=-%cpu | head -10
ps aux --sort=-%mem | head -10
```

### Limit processes (ulimit)
```bash
ulimit -a                  # show limits
ulimit -n 4096             # increase open files limit
# permanent in /etc/security/limits.conf
```

### Tuning kernel parameters for performance
Example for high network load (`/etc/sysctl.conf`):
```
net.core.somaxconn = 1024
net.ipv4.tcp_tw_reuse = 1
vm.swappiness = 10
```

---

## 16. Log Management and Journalctl

### Traditional logs (/var/log)
Important log files:  
- `/var/log/syslog` or `/var/log/messages` – general system log  
- `/var/log/auth.log` – authentication attempts  
- `/var/log/kern.log` – kernel messages  
- `/var/log/nginx/access.log` – web server access  

### journalctl (systemd)
```bash
journalctl                   # all logs
journalctl -u nginx          # logs for specific service
journalctl -f                # follow (tail)
journalctl --since "2025-01-01" --until "2025-01-02"
journalctl -p err            # priority: emerg, alert, crit, err, warning, notice, info, debug
journalctl -b -1             # previous boot logs
journalctl --disk-usage
```

### Log rotation (logrotate)
Configuration in `/etc/logrotate.conf` and `/etc/logrotate.d/`  
Example:
```
/var/log/nginx/*.log {
    weekly
    rotate 4
    compress
    delaycompress
    missingok
    notifempty
    create 0640 nginx adm
    sharedscripts
    postrotate
        systemctl reload nginx > /dev/null
    endscript
}
```

---

## 17. Databases on Linux (MySQL/PostgreSQL)

### MySQL / MariaDB
```bash
sudo apt install mysql-server
sudo mysql_secure_installation
```

**Connect and basic commands:**
```bash
mysql -u root -p
```
```sql
SHOW DATABASES;
CREATE DATABASE mydb;
USE mydb;
CREATE TABLE users (id INT, name VARCHAR(50));
INSERT INTO users VALUES (1, 'Alice');
SELECT * FROM users;
DROP DATABASE mydb;
```

**Backup & restore:**
```bash
mysqldump -u root -p mydb > backup.sql
mysql -u root -p mydb < backup.sql
```

### PostgreSQL
```bash
sudo apt install postgresql
sudo -u postgres psql
```
```sql
\l                       -- list databases
CREATE DATABASE mydb;
\c mydb;
CREATE TABLE test (id serial PRIMARY KEY, data text);
INSERT INTO test (data) VALUES ('hello');
SELECT * FROM test;
```

**Backup:**
```bash
pg_dump -U postgres mydb > mydb.sql
psql -U postgres mydb < mydb.sql
```

### Database tuning for Linux
- Increase `innodb_buffer_pool_size` (MySQL)  
- `shared_buffers` and `effective_cache_size` (PostgreSQL)  
- Use monitoring tools: `mytop`, `pg_top`

---

## 18. Backup and Recovery Strategies

### Simple backup with rsync
```bash
rsync -avz /source/ user@remote:/dest/
rsync -av --delete /home/ /backup/home/   # mirror deletion
```

### tar (tape archive)
```bash
tar -czvf backup.tar.gz /home/bob/Documents    # create
tar -xzvf backup.tar.gz -C /restore/path       # extract
```

### dd (disk clone / backup)
```bash
dd if=/dev/sda of=/backup/sda.img bs=4M status=progress
dd if=/backup/sda.img of=/dev/sda bs=4M
```

### Backup databases automatically (cron script)
Example: daily MySQL dump
```bash
#!/bin/bash
BACKUP_DIR="/backup/mysql"
DATE=$(date +%Y%m%d)
mysqldump --all-databases > "$BACKUP_DIR/all-$DATE.sql"
find $BACKUP_DIR -type f -mtime +7 -delete   # keep 7 days
```

### System recovery tips
- Boot from live USB, mount root, chroot to repair  
- GRUB rescue: `set root=(hd0,msdos1)`, `linux /vmlinuz ...`, `initrd /initrd.img`, `boot`  
- Single user mode: add `single` or `init=/bin/bash` to kernel command line

---

## 19. Troubleshooting Common Issues

### System won't boot
- Check logs via live CD  
- Rebuild initramfs: `dracut -f` or `update-initramfs -u`  
- Fix GRUB: `grub-install /dev/sda`, `update-grub`

### "Permission denied"
- Check file permissions (`ls -l`)  
- Check if filesystem mounted `noexec`  
- SELinux/AppArmor blocking  

### Disk full but no large files
- Deleted files still held open: `lsof | grep deleted` → restart process  
- Inode exhaustion: `df -i`

### High load average
- Check I/O wait: `iostat -x 1` → if %iowait high, disk bottleneck  
- Runaway processes: `top`, look for D state (uninterruptible sleep)  
- Memory pressure: `vmstat`, see si/so swapping

### Network unreachable
- `ip a` → interface up?  
- `ping 8.8.8.8` → if works but no DNS → check `/etc/resolv.conf`  
- `ss -tulpn` → service listening?  
- Firewall blocking: `iptables -L -n` or `ufw status`

### Can't SSH
- Check sshd is running: `systemctl status ssh`  
- Check firewall: `sudo ufw allow 22`  
- Verify `PermitRootLogin`, `PasswordAuthentication`  
- Look at `/var/log/auth.log` for refused messages

---

## 20. Advanced Topics – Kernel, Systemd, Containers

### Kernel modules
```bash
lsmod                      # list loaded modules
modprobe nvidia            # load module
modprobe -r nvidia         # remove
modinfo e1000              # show module info
```
Permanent module config: `/etc/modules-load.d/` and `/etc/modprobe.d/`

### Systemd deep dive
**Units:** `.service`, `.timer`, `.socket`, `.target`, `.mount`  
**Creating a custom service**  
`/etc/systemd/system/myapp.service`:
```
[Unit]
Description=My custom app
After=network.target

[Service]
Type=simple
User=bob
ExecStart=/usr/bin/python3 /home/bot/bot.py
Restart=on-failure

[Install]
WantedBy=multi-user.target
```
Then:
```bash
sudo systemctl daemon-reload
sudo systemctl enable --now myapp
```

### Containers (Docker / Podman)
```bash
# Install Docker
sudo apt install docker.io
sudo usermod -aG docker $USER

# Basic commands
docker pull nginx
docker run -d -p 8080:80 --name mynginx nginx
docker ps
docker exec -it mynginx bash
docker stop mynginx
docker rm mynginx
```

**Dockerfile example:**
```dockerfile
FROM ubuntu:22.04
RUN apt update && apt install -y python3
COPY app.py /app/
CMD ["python3", "/app/app.py"]
```
```bash
docker build -t myapp .
docker run myapp
```

### Init systems (historical)
- SysV init (old): scripts in `/etc/init.d/`
- Upstart (Ubuntu 6.04‑14.04)
- **systemd** (modern standard)

### Compiling a custom kernel (advanced)
```bash
make menuconfig
make -j $(nproc)
make modules_install
make install
```

---

## 21. Interview Prep – Senior Linux Role

### Common senior-level questions & concepts

**1. Troubleshooting boot failure with LVM and corrupted filesystem**  
- Boot from rescue media, activate LVM (`vgchange -ay`), run `fsck`.

**2. Explain inode, soft link, hard link**  
- Inode = metadata (permissions, pointers to data blocks).  
- Hard link = another directory entry pointing to same inode; can't cross filesystems.  
- Soft link = special file containing path; can point to directories and cross FS.

**3. How to debug high CPU or memory usage without top/htop?**  
- Use `ps`, `/proc`, `vmstat`, `strace -p PID`, `perf top`.

**4. Difference between fork() and exec()**  
- `fork()` creates child process as copy of parent.  
- `exec()` replaces current process with new program.

**5. Linux startup process (UEFI/BIOS → GRUB → Kernel → init/systemd)**  
- UEFI/BIOS → bootloader (GRUB) loads kernel → kernel mounts initramfs → starts systemd (PID 1) → targets.

**6. How to limit a process's CPU or memory?**  
- `cpulimit`, `ulimit`, cgroups (`systemd-run --scope -p CPUQuota=50% ...`)

**7. Recover a deleted file that is still open**  
- `lsof | grep deleted` → get PID/fd → `cp /proc/PID/fd/FD /recovered/file`.

**8. Explain RAID levels and LVM snapshots**  
- RAID 0,1,5,6,10; LVM snapshots for point‑in‑time copy.

**9. How does `sudo` work?**  
- Setuid binary `/usr/bin/sudo`, reads `/etc/sudoers`, logs to syslog.

**10. Linux security: chroot, namespaces, seccomp**  
- chroot: change root directory for a process.  
- Namespaces: isolate processes (PID, net, mount).  
- seccomp: syscall filtering.

**11. Performance tuning for a web server**  
- Kernel: increase `net.core.somaxconn`, `net.ipv4.tcp_tw_reuse`.  
- Nginx: worker processes, keepalive.  
- Limit open files, use SSD.

**12. How to find which process listens on port 80**  
- `ss -tulpn | grep :80` or `lsof -i :80`.

**13. Write a script to rotate logs with compression**  
```bash
#!/bin/bash
mv /var/log/app.log /var/log/app-$(date +%Y%m%d).log
gzip /var/log/app-*.log
kill -HUP $(cat /var/run/app.pid)
```

**14. Explain systemd target vs runlevel**  
- Runlevel 3 = multi-user.target, runlevel 5 = graphical.target.

**15. Automate deployment of SSH keys to 100 servers**  
- Ansible, or `for i in $(cat servers); do ssh-copy-id user@$i; done`

**16. What is `/proc` and `/sys`?**  
- `/proc`: process and kernel information (virtual).  
- `/sys`: device and driver info (sysfs).

**17. How to limit user to run only certain commands?**  
- Use `rssh` or `rbash` (restricted shell) or `sudo` with command restrictions.

**18. Explain bonding/teaming of network interfaces**  
- Combine multiple NICs for redundancy/throughput: mode 0 (balance‑rr), mode 1 (active‑backup), etc.

**19. How to check if a service failed to start and why?**  
- `systemctl status service` → see log lines; `journalctl -u service -xe`.

**20. What is the difference between `kill` and `pkill`?**  
- `kill` uses PID; `pkill` uses process name pattern.

### Practical senior tasks
- Set up a highly available Nginx load balancer with keepalived.  
- Write a systemd timer that runs a backup script and emails report.  
- Configure a chroot environment for SFTP users.  
- Performance benchmark using `sysbench` or `fio`.  
- Automate patching of 50 servers with ansible.  

---

## 22. Cheat Sheets & Quick References

### Common commands quick reference

| Category | Command | Example |
|----------|---------|---------|
| Files | `ls -la`, `cp -r`, `mv`, `rm -rf` | `ls -la /home` |
| Permissions | `chmod 755`, `chown user:group` | `chmod +x script.sh` |
| Text | `grep`, `sed`, `awk`, `sort`, `uniq` | `ps aux | grep nginx` |
| Processes | `ps aux`, `top`, `kill -9 PID` | `pkill -f python` |
| Networking | `ip a`, `ss -tulpn`, `ping` | `ss -tulpn \| grep :80` |
| Disk | `df -h`, `du -sh`, `fdisk -l` | `du -sh /var/*` |
| Users | `useradd`, `passwd`, `usermod` | `sudo useradd -m -G sudo bob` |
| Systemd | `systemctl start/stop/status` | `systemctl status sshd` |
| Logs | `journalctl -u`, `tail -f` | `journalctl -f` |
| Archiving | `tar czf`, `tar xzf` | `tar czf backup.tar.gz /home` |
| Download | `wget`, `curl -O` | `curl -O https://file.zip` |
| SSH | `ssh user@host`, `scp` | `scp file.txt server:/tmp/` |

### Useful key sequences
- `Ctrl+C` – interrupt current command  
- `Ctrl+Z` – suspend current job  
- `Ctrl+D` – EOF / logout  
- `Ctrl+L` – clear screen  
- `Ctrl+R` – reverse search in history  
- `!!` – repeat last command  
- `!$` – last argument of previous command  

### Environment files order (login shell)
1. `/etc/profile`  
2. `~/.bash_profile` (or `~/.bash_login`, `~/.profile`)  
3. `~/.bashrc` (usually sourced from `.bash_profile`)

### Performance tools table

| Tool | Use | Example |
|------|-----|---------|
| `top` / `htop` | real-time processes | `htop` |
| `vmstat 2` | system memory, CPU, IO | `vmstat 2 10` |
| `iostat -x 1` | disk I/O | `iostat -x 1` |
| `sar -u 1` | historical CPU | `sar -u 1 3` |
| `netstat -i` | interface stats | `netstat -i` |
| `ss -tunap` | socket stats | `ss -tunap` |
| `strace -p PID` | trace syscalls | `strace -p 1234` |
| `lsof` | open files | `lsof -i :80` |

### Rescue and repair
- `fsck /dev/sda1` – check filesystem (unmount first)  
- `grub-install /dev/sda` – reinstall GRUB  
- `dpkg --configure -a` (Debian) – fix broken packages  
- `rpm -Va` (RHEL) – verify all packages

---

## Final words

This document covers everything from `cd` to kernel tuning, from `sudo` to containers. To truly master Linux:
- **Practice every command** – set up a lab with VirtualBox or cloud VMs.
- **Break things and fix them** – the best learning.
- **Read logs** – `/var/log` and `journalctl` are your best friends.
- **Automate everything** – scripts, cron, Ansible.
- **Contribute to open source** – fix a bug or write documentation.

A senior Linux professional is not someone who remembers all flags, but someone who knows how to find answers, understands the system’s internals, and can design reliable, automated solutions. Good luck on your journey!

```

You can now copy the entire block above and save it as `linux_complete_guide.md`. It contains over 500 lines of structured notes, ready to use.