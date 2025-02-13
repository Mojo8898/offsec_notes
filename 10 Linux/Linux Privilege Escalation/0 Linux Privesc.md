# Methodology

## Enumeration

```bash
# Change directories to RAM so it is cleared on reboot
cd /dev/shm

# Run automated tools if possible
.\linpeas.sh
.\pspy64

# Gather general information
id
cat /etc/passwd
hostname

# Check sudo permissions, utilize GTFOBins to check any non-default configurations
sudo -l

# Check for SUID binaries
find / -perm -4000 -ls 2>/dev/null

# Find files owned by target user/group
find / -user $USER -type f -exec ls -ldb {} \; 2>/dev/null
find / -group $GROUP -type f -exec ls -ldb {} \; 2>/dev/null

# Find writable files
find / '(' -type f -or -type d ')' '(' '(' -user $USER ')' -or '(' -perm -o=w ')' ')' 2>/dev/null | grep -v '/proc/' | grep -v $HOME | sort | uniq
for g in `groups`; do find \( -type f -or -type d \) -group $g -perm -g=w 2>/dev/null | grep -v '/proc/' | grep -v $HOME; done

# Check commonly sensitive directories
ls /opt
ls /var/www

# Check processes
ps -ef
watch -n 1 "ps -aux | grep $STRING"

# Check Network information
netstat -tunlp
ip a
routel

# Check kernel/OS info
uname -a
cat /etc/issue
cat /etc/os-release
cat /etc/lsb-release

# Check iptables
cat /etc/iptables/rules.v4

# Check crontabs
cat /etc/crontab
crontab -l
ls -alh /etc/cron*
cat /etc/cron.d/example_cron

# List installed applications
dpkg -l

# Check for unmounted drives
ls /dev 2>/dev/null | grep -i "sd"
cat /etc/fstab 2>/dev/null | grep -v "^#" | grep -Pv "\W*\#" 2>/dev/null
lsblk

# List kernel module information
lsmod
/sbin/modinfo $MODULE

# Check environment variables
env

# Check .bashrc
cat ~/.bashrc

---

# Find capabilities
/usr/sbin/getcap -r / 2>/dev/null

# Inpsect incoming traffic for the string "pass"
sudo tcpdump -i lo -A | grep "pass"

# Check environment variables
env

# Check $PATH
echo $PATH

# Check timers
systemctl list-timers --all

# Check SGID binaries
find / -user root -perm -6000 -exec ls -ldb {} \; 2>/dev/null
```

## Tools

1. [linpeas.sh](https://github.com/carlospolop/PEASS-ng/releases/)
2. [pspy64](https://github.com/DominicBreuker/pspy/releases)
