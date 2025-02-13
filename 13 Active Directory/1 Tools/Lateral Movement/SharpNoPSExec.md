---
tags:
  - tool
---
# SharpNoPSExec

[SharpNoPSExec](https://github.com/juliourena/SharpNoPSExec) is a tool designed to facilitate lateral movement by leveraging existing services on a target system without creating new ones or writing to disk, minimizing detection risk.

## Capabilities

```powershell
# Connect
.\SharpNoPSExec.exe --target=$IP --payload="c:\windows\system32\cmd.exe /c $CMD"
```
