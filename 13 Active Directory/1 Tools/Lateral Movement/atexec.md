---
tags:
  - tool
---
# atexec

The [atexec.py](https://github.com/fortra/impacket/blob/master/examples/atexec.py) script utilizes the Windows Task Scheduler service, which is accessible through the `atsvc` SMB pipe. It enables us to remotely append a task to the scheduler, which will execute at the designated time.

## Capabilities

```powershell
# Execute command
impacket-atexec $DOMAIN/$USER:$PASSWD@$IP "powershell -e ..."
```
