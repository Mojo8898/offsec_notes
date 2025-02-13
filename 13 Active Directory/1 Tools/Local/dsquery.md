---
tags:
  - tool
---
# dsquery

A tool installed by default on any host with the `Active Directory Domain Services Role` installed for enumerating active directory.

## Capabilities

```powershell
# User search
dsquery user

# Computer search
dsquery computer

# Wildcard search
dsquery * "CN=Users,$DOMAIN_DN"

# Users with specific attributes set (PASSWD_NOTREQD)
dsquery * -filter "(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))" -attr distinguishedName userAccountControl

# Domain controller search
dsquery * -filter "(userAccountControl:1.2.840.113556.1.4.803:=8192)" -limit 5 -attr sAMAccountName
```
