# Linux System Administrator Interview Practice 🚀

## Overview

This repository contains scenario-based Linux System Administrator interview preparation questions and troubleshooting approaches.

Topics covered:

- Linux Boot Process
- Service Troubleshooting
- Filesystem Issues
- LVM Management
- SSH Troubleshooting
- User Management
- Sudo Access
- SELinux Troubleshooting
- Disk Management
- Performance Troubleshooting
- NFS Issues
- DNS Troubleshooting
- Log Management
- Kernel Issues
- Server Recovery Scenarios

---

# 1. Application Service Down Troubleshooting

## Scenario

Application team reports that the application is down.

## Troubleshooting Steps

Check service status:

```bash
systemctl status service_name

If service is stopped:

systemctl restart service_name

Check service logs:

journalctl -u service_name

Check listening ports:

ss -tunlp | grep service_name

Check firewall:

firewall-cmd --list-all
2. Filesystem Space Issue
Scenario

Filesystem is full.

Check disk usage:

df -h

Find large files:

du -sh /* | sort -h

Check application logs:

du -sh /var/log/*

Solutions:

Rotate old logs
Compress unused logs
Remove unnecessary files
Extend filesystem if LVM is used
3. LVM Disk Extension
Extend Logical Volume

Check existing volumes:

vgs
lvs

Extend LV:

lvextend -L +5G -r /dev/vgname/lvname

Example:

lvextend -L +100G -r /dev/vgname/lvname

Verify:

df -h
4. SSH Connection Issue
Scenario

Unable to access server through SSH.

Check SSH service:

systemctl status sshd

Restart if required:

systemctl restart sshd

Check SSH port:

ss -tunlp | grep ssh

Check firewall:

firewall-cmd --list-all
5. High CPU Utilization
Troubleshooting

Check load:

top

Check load average:

uptime

Find high CPU processes:

ps -eo pid,cmd,%cpu --sort=-%cpu | head

Check memory:

free -m

Check disk:

df -h

Before killing process:

Confirm with application team
Check impact
6. Memory Utilization Issue

Check memory:

free -m

Check processes:

ps -eo pid,cmd,%mem --sort=-%mem | head

Check swap:

swapon -s

Enable swap:

swapon -a
7. Apache/HTTPD Service Issue

Check service:

systemctl status httpd

Check port:

ss -tunlp | grep httpd

Check firewall:

firewall-cmd --list-all
8. ZFS Pool Issue

Check pool status:

zpool status -x

If disk failure:

Verify hardware using iDRAC/iLO
Check disk status
Raise hardware vendor ticket
9. Linux Boot Process
Boot Flow
Power ON
   |
BIOS/UEFI
   |
POST Hardware Check
   |
GRUB/GRUB2
   |
Kernel Loading
   |
initramfs
   |
systemd
   |
Services
   |
Login Screen

Check kernel:

uname -r

Check init process:

ps -p 1
10. Kernel Boot Failure
Scenario

Server fails after kernel update.

Steps:

Access console using iDRAC/iLO
Enter GRUB menu
Select previous kernel
Boot server
Investigate failed kernel

Check kernels:

ls /boot
11. File Permission Issue

Check permissions:

ls -l filename

Check ownership:

ls -ld directory

Verify with administrator before changing permissions.

Change permissions:

chmod
chown
12. User Creation and SSH Key Setup

Create user:

adduser appuser

Add group:

usermod -aG applicationgroup appuser

Create SSH directory:

mkdir /home/appuser/.ssh

Copy public key:

cp public.key /home/appuser/.ssh/authorized_keys

Permissions:

chmod 700 ~/.ssh
chmod 600 ~/.ssh/authorized_keys
13. Passwordless SSH Authentication

Generate SSH key:

ssh-keygen

Copy public key:

scp public.key root@server:/home/user/.ssh/authorized_keys

Enable:

vi /etc/ssh/sshd_config

Check:

PubkeyAuthentication yes

Restart:

systemctl restart sshd
14. Zombie Process Troubleshooting

Zombie process:

Process completed execution
Entry still exists in process table

Check:

ps aux | grep Z

Find parent PID:

ps -ef

Restart parent process if required.

15. SELinux Troubleshooting

Check status:

getenforce

Check denials:

ausearch -m AVC

Check context:

ls -Z /var/www/html

Restore context:

restorecon -Rv /var/www/html

Enable Apache network access:

setsebool -P httpd_can_network_connect on
16. Disk Monitoring Script

Example:

#!/bin/bash

THRESHOLD=80

LOG_FILE="/var/log/disk_monitor.log"

CURRENT_DATE=$(date '+%Y-%m-%d %H:%M:%S')

DISK_USAGE=$(df / | awk 'NR==2 {print $5}' | tr -d '%')

if [ "$DISK_USAGE" -ge "$THRESHOLD" ]
then
echo "$CURRENT_DATE WARNING - Disk usage is ${DISK_USAGE}%" >> $LOG_FILE
exit 1
fi
17. Cron Job Troubleshooting

Check cron:

crontab -l

Check script permission:

ls -l script.sh

Check logs:

grep CRON /var/log/cron
18. Log Management

Check logs:

du -sh /var/log/*

Preferred solution:

Use logrotate.

Configuration:

/etc/logrotate.d/application

Example:

/var/log/application/app.log {
daily
rotate 7
compress
missingok
copytruncate
}
19. NFS Mount Issue

Check mount:

df -h

Check NFS:

showmount -e server_ip

Mount manually:

mount -t nfs server:/share /mountpoint

Permanent mount:

vi /etc/fstab

Example:

server:/share /data nfs defaults,_netdev 0 0

Test:

mount -a
20. DNS Troubleshooting

Check DNS configuration:

cat /etc/resolv.conf

Test:

nslookup hostname

or:

dig hostname

Check hosts file:

cat /etc/hosts
21. Unexpected Server Reboot Investigation

Check reboot history:

last reboot

Check previous boot logs:

journalctl -b -1

Kernel logs:

journalctl -k -b -1

Check OOM:

journalctl | grep -i "out of memory"

Check sudo activity:

grep sudo /var/log/secure
22. Performance Troubleshooting
CPU
top
sar -u
Memory
free -m
Disk
df -h
iostat -xz
Processes
ps aux --sort=-%cpu
23. Emergency Mode Recovery

Check failures:

systemctl --failed

Boot logs:

journalctl -xb

Check fstab:

cat /etc/fstab

Check filesystem:

fsck

Rollback option:

Restore snapshot
Boot previous kernel
Recover system
