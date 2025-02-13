---
tags:
  - tool
---
# ssh

It's ssh, cmon now.

## Capabilities

```powershell
# Test credentials with nxc
nxc ssh $IP -u $USER -p '$PASSWD'

# Connect with private key
ssh -i C:\$PRIVATE_KEY -l $USER@$DOMAIN -p 22 $HOSTNAME powershell.exe
```

**Note:** If connection fails due to improper private key permissions, we just need to copy it to a file our current user owns. As `BUILTIN\Users` cannot have `(I)(RX)` on it.

```powershell
copy C:\$PRIVATE_KEY C:\Users\$USER\
```

**Note:** Utilizing PKI authentication over SSH to a Windows host won't allow us to perform network based authentication, limiting us to local interaction.
