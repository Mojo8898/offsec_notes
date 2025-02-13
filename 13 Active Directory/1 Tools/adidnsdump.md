---
tags:
  - tool
---
# adidnsdump

Enumerate DNS records in a domain as a domain user.

## Capabilities

```powershell
# Run DNS query
adidnsdump -u $DOMAIN\\$USER ldap://$DC

# Resolve hidden records (seen as ? symbols)
adidnsdump -u $DOMAIN\\$USER ldap://$DC -r
```

**Note:** Records are output to a `records.csv` file.
