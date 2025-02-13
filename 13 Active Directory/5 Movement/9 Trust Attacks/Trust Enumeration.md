# Trust Enumeration
## Windows

```powershell
# Using ActiveDirectory module
Import-Module ActiveDirectory
Get-ADTrust -Filter *
Get-ADTrust -Identity $DOMAIN

# Using PowerView
Import-Mdoule .\Powerview.ps1
Get-DomainTrust
Get-DomainTrustMapping

# Check if SID filtering is enabled (set to true if so, almost never enabled cross forests though)
Get-ADTrust -Identity $DOMAIN | select name,sidfilteringquarantined
```

**Note:** The difference between `Get-DomainTrust` and `Get-DomainTrustMapping` is the ladder performs `Get-DomainTrust` and then also attempts to enumerate all trusts for every domain that is uncovered.

REDACTED THEORY