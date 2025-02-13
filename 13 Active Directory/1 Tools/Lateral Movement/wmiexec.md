---
tags:
---
# wmiexec

Executes a semi-interactive shell using WMI over port 445.

Requires `ADMIN$` share to be available. Only works against Active Directory domain accounts and the built-in local administrator account.

## Capabilities

```powershell
# Authenticate with credentials
wmiexec.py $DOMAIN/$USER:'$PASSWD'@$IP

# Authenticate with kerberos
wmiexec.py $DOMAIN/$USER@$IP -k -no-pass

# Authenticate with hash
wmiexec.py $USER@$IP -hashes :$NTHASH 
```
