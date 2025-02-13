---
tags:
  - tool
  - windows
  - lateral_movement
---
# runas

Run commands as other users. Creates a new window and therefore requires RDP.

## Capabilities

```powershell
# Execute cmd in another user's context
runas /user:$USER cmd

# Execute powershell in the current user's context
runas /netonly /user:$DOMAIN\$USER powershell
```

**Note:** `/netonly` tells Windows to use the provided credentials for remote connections only. This means that the credentials will be used to authenticate to the domain controller, but not to the local machine.