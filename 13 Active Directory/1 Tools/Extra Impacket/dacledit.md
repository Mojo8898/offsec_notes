---
tags:
  - tool
---
# dacledit

Enumerate DACLs

## Capabilities

```powershell
# Read DACL
dacledit.py -target $PRINCIPAL $DOMAIN/$USER:'$PASSWD'

# Read permissions that principal has over target object
dacledit.py -principal $PRINCIPAL -target '$TARGET_OBJ' $DOMAIN/$USER:'$PASSWD'

# Grant full control over target object to principal
dacledit.py -principal $PRINCIPAL -target '$TARGET_OBJ' -action write $DOMAIN/$USER:'$PASSWD'
```
