---
tags:
  - tool
---
# netdom

Retrieve domain information, including a list of workstations, servers, and domain trusts.

## Capabilities

```powershell
# Query domain trusts
netdom query /domain:$DOMAIN trust

# Query domain controllers
netdom query /domain:$DOMAIN dc

# Query workstations and servers
netdom query /domain:$DOMAIN workstation
```
