---
tags:
  - tool
---
# crontab

Manage the crontab for users, root, and system

## Capabilities

```powershell
# Edit the current users crontab
crontab -e

# Edit the root users crontab
sudo crontab -e
```

## Info

Crontabs are executed using the following template.

```
<minute> <hour> <day/month> <month> <day/week> <command>
```

On top of crontabs existing for each user and root, system crontabs can be found in the following places:

```powershell
# System
/etc/crontab
/etc/cron.hourly
/etc/cron.daily
/etc/cron.d/

# OS specific
/var/spool/cron/crontabs/* # Ubuntu/Debian
/var/spool/cron/* # RHEL, CentOS, Amazon Linux, Suse
```
