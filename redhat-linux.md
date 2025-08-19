# RedHat Linux Notes 🐧

Comprehensive reference of RedHat Enterprise Linux (RHEL) commands and concepts.  
Covers **file management, users & groups, processes, packages, networking, monitoring, LVM, systemctl, SELinux, Podman**, and more.

---


## 🔹 Basic Commands
- `whoami` → Shows current logged-in user  
- `pwd` → Prints working directory  
- `ls` → Lists files and directories  
- `cd <dir>` → Change directory  
- `mkdir <dir>` → Create directory  
- `rmdir <dir>` → Remove directory  

## 🔹 File Operations
- `touch file.txt` → Create empty file  
- `cp file1 file2` → Copy file  
- `mv file1 file2` → Move/Rename file  
- `rm file.txt` → Remove file  

## 🔹 Viewing File Content
- `cat file.txt` → Show full content  
- `less file.txt` → View content (scrollable)  
- `more file.txt` → View content page by page  
- `head file.txt` → Show first 10 lines  
- `tail file.txt` → Show last 10 lines  
- `tail -f file.txt` → Live monitor file (useful for logs)  

## 🔹 User & Permissions
- `who` → Show logged-in users  
- `id` → Show user ID & groups  
- `chmod 755 file.txt` → Change permissions  
- `chown user:group file.txt` → Change ownership  

## 🔹 Process Management
- `ps` → Show running processes  
- `top` → Show active processes in real-time  
- `kill <PID>` → Kill a process  
- `uptime` → Show system uptime  

## 🔹 Networking
- `ping google.com` → Test connectivity  
- `ifconfig` or `ip a` → Show IP address  
- `netstat -tulnp` → Show open ports  

---

## 📂 File & Directory Management
```bash
pwd                        # Print current directory
ls -l                      # List files with details
lsblk                      # Show block devices
cd /path                   # Change directory
touch file.txt             # Create blank file or update timestamp
cat file.txt               # Display file contents
wc file.txt                # Word/line/character count
file filename              # Show file type based on data, not extension
du -h filename             # Show disk usage in human readable form
find / -name "*.txt"       # Search files
grep "^root" filename      # Find lines starting with 'root'
grep "nologin$" filename   # Find lines ending with 'nologin'
env                        # Show environment variables
````

---

## 👥 User & Group Management

### Types of Users

* **Super user**: root (`uid=0`, shell: `/bin/bash`)
* **System users**: services like mail, ftp, apache (`/sbin/nologin`)
* **Local users**: normal users (`uid=1000-60000`, shell: `/bin/bash`)

### Commands

```bash
useradd devuser                # Add new user
passwd devuser                 # Set password
id devuser                     # Show user info
groups devuser                 # Show groups
su - username                  # Switch user
usermod -aG wheel devuser      # Modify user groups
gpasswd groupname              # Add password to group
gpasswd -d ironman c           # Remove user from group
newgrp sysadmin                # Change primary group temporarily
```

### User Data Files

* `/etc/passwd` → user account info
* `/etc/shadow` → passwords + aging
* `/etc/group` → groups info
* `/etc/gshadow` → secure group data
* `/etc/skel` → skeleton files for new users
* `/var/spool/mail` → user mail storage
* `/home/username` → user home directory

---

## 🔐 Sudoers (3 ALLs)

In `/etc/sudoers`:

1. **First ALL** → Hostname
2. **Second ALL** → Target User
3. **Third ALL** → Allowed Commands

Example:

```
username ALL=(ALL) ALL
```

---

## ⚙️ Permissions

```bash
chmod 755 file.sh            # Change permissions
chown user:group file.sh     # Change owner
umask                        # Show default mask
umask 022                    # Change temporarily
vim ~/.bashrc                # Change permanently
bash                         # Reload changes
```

### Special Permissions

* **SUID = 4**
* **SGID = 2**
* **Sticky bit = 1**

---

## 🖥️ Process & Job Management

```bash
ps aux                        # Show all processes
top                           # Live process monitoring
uptime                        # Show system uptime
jobs                          # List background jobs
fg %1                         # Bring job 1 to foreground
bg %1                         # Send job 1 to background
CTRL+Z                        # Suspend job
kill -l                       # Show all signals
kill -9 <pid>                 # Kill process
pkill httpd                   # Kill by name
```

* **PID** = Process ID
* **PPID** = Parent Process ID
* **systemd** = PID 1 (replaces init in RHEL 7+)
* **Daemon process** = Background service (e.g., sshd)

---

## 🔧 Systemd & Runlevels

```bash
systemctl get-default          # Check default target (CLI/GUI)
systemctl set-default multi-user.target   # Set CLI mode
systemctl set-default graphical.target    # Set GUI mode
startx                         # Start GUI manually
```

### Runlevels vs Targets

* 0 → shutdown
* 1 → single-user mode
* 2 → multi-user (no networking)
* 3 → CLI multi-user
* 5 → GUI
* 6 → reboot

---

## 🌐 Networking, SSH & Firewall

```bash
ssh user@ip                   # SSH to remote system
sshd                          # SSH daemon
firewall-cmd --permanent --add-port=82/tcp
systemctl restart firewalld
systemctl mask firewalld       # Disable service
systemctl unmask firewalld
```

---

## 📦 Package Management (RPM & YUM/DNF)

### RPM

```bash
rpm -q coreutils              # Check package
rpm -qi coreutils             # Detailed info
rpm -ql coreutils             # List package files
rpm -qc coreutils             # Show config files
rpm -qd coreutils             # Documentation
rpm -qa                       # List all packages
rpm -qf /etc/passwd           # Find package owning file
rpm -e wget                   # Remove package
rpm -ivh pkg.rpm              # Install
rpm -Uvh pkg.rpm              # Update
```

⚠️ **RPM Limitations**: No dependency tracking, no system-wide updates.

### YUM/DNF

```bash
yum install httpd -y          # Install package
yum remove httpd              # Remove package
yum repolist                  # Show repos
yum repoinfo                  # Repo details
yum history                   # Show history
```

Repo config example:

```ini
[AppStream]
baseurl=file:///path/to/local
gpgcheck=0
enabled=1
name=AppStream
```

---

## 🕒 Time & NTP

```bash
chronyc sources -v            # Show NTP sources
```

* `iburst` → speeds up initial sync

---

## 📂 Archiving & Backup

```bash
tar -cvf backup.tar /etc       # Tar archive
tar -cvzf backup.gz /etc       # Gzip
tar -cvjf backup.bz2 /etc      # Bzip2
tar -cvJf backup.xz /etc       # XZ
tar -xvf backup.tar            # Extract
tar -C /usr -xvf backup.tar    # Extract to /usr
```

---

## 💾 Storage Management

### Disk & Partition

```bash
lsblk -f                      # Show UUIDs
blkid                         # Show block IDs
wipefs -af /dev/sda           # Wipe filesystem
fdisk -l                      # Show partitions
partprobe                     # Reload partition table
```

### LVM

Steps: Partition → PV → VG → LV → FS → Mount

```bash
df -h                         # Show disk usage
lvextend -r -L +1G /dev/vg/lv # Extend LV and filesystem
lvreduce -L -500M /dev/vg/lv  # Reduce LV
resize2fs /dev/vg/lv          # Resize ext4
xfs_growfs /mountpoint        # Resize XFS
```

### Stratis (Storage Management)

```bash
stratis pool create mypool /dev/sdd /dev/sde
stratis pool list
stratis filesystem create mypool myfs
stratis filesystem list
```

---

## 📜 Logs & Auditing

```bash
tail -f /var/log/messages      # Live logs
last                           # Show login history
lastb                          # Show failed logins
w                              # Show logged-in users
uptime                         # Show uptime
```

* Logs: `/var/log/`
* **rsyslog** → text logs
* **systemd-journald** → binary logs (`/var/log/journal/`)

---

## 🔒 SELinux

```bash
ls -lZ /var/www/html           # Show SELinux context
chcon -t httpd_sys_content_t file1
systemctl restart httpd
systemctl enable httpd
```

---

## 🐳 Podman (Containers on RHEL)

```bash
podman login registry.redhat.io
podman pull image-name
podman images
podman run -d --name myapp image-name
podman ps
podman stop myapp
podman rm myapp
```

Generate systemd unit:

```bash
podman generate systemd --name myapp --new --files
systemctl --user daemon-reload
systemctl --user start container-myapp.service
loginctl enable-linger username
```

---

## 🔑 Root Password Reset

1. At boot → `e` → add `rd.break` → `CTRL+X`
2. Remount:

   ```bash
   mount -o remount,rw /sysroot
   chroot /sysroot
   passwd
   touch /.autorelabel
   exit
   exit
   ```

---

