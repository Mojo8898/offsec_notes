---
tags:
  - tool
---
# winrs

 A command-line tool that allows us to execute commands on remote system via WinRM.

## Capabilities

```powershell
# Test command execution
winrs -r:$HOST "powershell -c whoami;hostname"

# Test with authentication
winrs /remote:$HOST /username:$USER /password:$PASSWD "powershell -c whoami;hostname"
```
