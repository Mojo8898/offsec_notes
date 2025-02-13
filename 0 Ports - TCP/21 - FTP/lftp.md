---
tags:
  - tool
---
# lftp

Connect via FTP with SSL/TLS

## Capabilities

```powershell
# Connect
lftp -u $USER,$PASS -e "set ftp:ssl-force true; set ftp:ssl-protect-data true; set ssl:verify-certificate no" $DOMAIN
```
