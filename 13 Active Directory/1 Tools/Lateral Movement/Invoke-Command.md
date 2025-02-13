---
tags:
  - tool
---
# Invoke-Command

A PowerShell command that allows us to execute commands on remote system via WinRM.

## Capabilities

```powershell
# Test command execution
Invoke-Command -ComputerName $HOSTNAME -ScriptBlock { hostname;whoami }

# Test with authentication
$username = "$NETBIOS\$USER"
$password = "$PASSWD"
$securePassword = ConvertTo-SecureString $password -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential ($username, $securePassword)
Invoke-Command -ComputerName $IP -Credential $cred -ScriptBlock { whoami;hostname }
```
