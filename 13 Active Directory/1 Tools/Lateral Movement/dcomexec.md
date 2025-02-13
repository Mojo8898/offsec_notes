---
tags:
  - tool
---
# dcomexec

Similar to `wmiexec.py`, but utilizes different `DCOM` endpoints for command execution.

## Capabilities

```powershell
# Standard connect
dcomexec.py -object MMC20 $DOMAIN/$USER:'$PASSWD'@$IP '$CMD' -silentcommand

# In the case TCP 445 is not available
dcomexec.py -object MMC20 $DOMAIN/$USER:'$PASSWD'@$IP '$CMD' -silentcommand -no-output
```

**Note:** We can sub out `MMC20` for `ShellWindows` and `ShellBrowserWindow` objects if they are enabled to allow us to perform code execution.
