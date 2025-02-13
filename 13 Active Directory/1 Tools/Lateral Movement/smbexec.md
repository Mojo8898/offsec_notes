---
tags:
  - tool
---
# smbexec

Execute remote commands exclusively over port 445. Also sets up a service, using only MSRPC for this, and manages the service through the `svcctl` SMB pipe.

## Capabilities

```powershell
# Connect
smbexec.py $DOMAIN/$USER:'$PASSWD'@$IP
```
