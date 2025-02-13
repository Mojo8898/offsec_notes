---
tags:
  - tool
---
# PowerShell

Enumerate AD with powershell.

## Capabilities

```powershell
# Import module
Import-Module ActiveDirectory

# Basic domain information
Get-ADDomain

# Query hostnames and IPs
Get-ADComputer -Filter * -Property DnsHostName | ForEach-Object { $_.DnsHostName } | Where-Object { $_ } | ForEach-Object { [pscustomobject]@{ ComputerName = $_; IPAddress = (Resolve-DnsName $_ -ErrorAction SilentlyContinue).IPAddress } } | Format-Table -AutoSize

# Test port availablility
Test-NetConnection -ComputerName $HOSTNAME -Port $PORT

# Query SPNs
Get-ADUser -Filter {ServicePrincipalName -ne "$null"} -Properties ServicePrincipalName

# Query account name from SPN
Get-ADUser -Filter {ServicePrincipalName -eq "$USER"} -Properties ServicePrincipalName

# Verify domain trust relationships
Get-ADTrust -Filter *

# Query groups
Get-ADGroup -Filter * | select name
Get-ADGroup -Identity "$GROUP"

# Query group members
Get-ADGroupMember -Identity "$GROUP"
Get-ADGroupMember -Identity "$GROUP" | select SamAccountName

# Find a user with a specified property that is also a member of a specified group
Get-ADGroupMember -Identity 'Protected Users' | Get-ADUser -Properties ServicePrincipalName, Memberof | Where-Object { $_.ServicePrincipalName -ne $NULL }

# Create a list of domain users
Get-ADUser -Filter * | Select-Object -ExpandProperty SamAccountName > ad_users.txt

# Look at ACL for a single user
(Get-ACL "AD:$((Get-ADUser $USER).distinguishedname)").access | ? {$_.IdentityReference -eq "$DOMAIN\$TARGET_USER"}

# Look for GenericAll over a target user
(Get-ACL "AD:$((Get-ADUser $USER).distinguishedname)").access | ? {$_.ActiveDirectoryRights -match "WriteProperty" -or $_.ActiveDirectoryRights -match "GenericAll"} | Select IdentityReference,ActiveDirectoryRights -Unique | ft -W

# Query ACE's for a user list
foreach($line in [System.IO.File]::ReadLines("C:\Users\$USER\Desktop\ad_users.txt")) {get-acl  "AD:\$(Get-ADUser $line)" | Select-Object Path -ExpandProperty Access | Where-Object {$_.IdentityReference -match '$DOMAIN\\$sAMAccountName'}}

# Get users with reversible encryption
Get-ADUser -Filter 'userAccountControl -band 128' -Properties userAccountControl

# Enumerate trust relationships
Get-ADTrust -Filter *

# Create a new PSCredential object with valid credentials for use with powerview
$SecPassword = ConvertTo-SecureString '$PASSWD' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('$DOMAIN\$USER', $SecPassword)

# Establish a WinRM session
$SecPassword = ConvertTo-SecureString '$PASSWD' -AsPlainText -Force
$Cred = New-Object System.Management.Automation.PSCredential('$DOMAIN\$USER', $SecPassword)
Enter-PSSession -ComputerName $COMPUTER -Credential $Cred

# Connect via IPv6
Enter-PSSession -ComputerName $IPv6 -Authentication Negotiate

# Query forest information
Get-ADForest

# Convert read file to Base64, super helpful on slow/unreliable connections
[Convert]::ToBase64String([System.IO.File]::ReadAllBytes("$FILE"))

# Discover service associated with pid from netstat
tasklist /svc /FI "PID eq $PID"

# Sync time with DC
w32tm /config /manualpeerlist:$DC_IP /syncfromflags:manual /reliable:YES /update
```

**If Group Policy Management Tools are installed:**

```powershell
# Enumerate GPOs
Get-GPO -All | Select DisplayName
```

## LDAP Filters

```powershell
# Filter disabled user accounts
Get-ADUser -LDAPFilter '(userAccountControl:1.2.840.113556.1.4.803:=2)' | select name

# Find groups that a user is a member of
Get-ADGroup -LDAPFilter '(member:1.2.840.113556.1.4.1941:=$USER_DN)' | select Name

# Filter user descriptions that exist
Get-ADUser -Properties * -LDAPFilter '(&(objectCategory=user)(description=*))' | select samaccountname,description

# Find trusted users
Get-ADUser -Properties * -LDAPFilter '(userAccountControl:1.2.840.113556.1.4.803:=524288)' | select Name,memberof, servicePrincipalName,TrustedForDelegation | fl

# Find trusted computers
Get-ADComputer -Properties * -LDAPFilter '(userAccountControl:1.2.840.113556.1.4.803:=524288)' | select DistinguishedName,servicePrincipalName,TrustedForDelegation | fl

# Find users with PASSWD_NOTREQD flag
Get-AdUser -LDAPFilter '(&(objectCategory=person)(objectClass=user)(userAccountControl:1.2.840.113556.1.4.803:=32))(adminCount=1)' -Properties * | select name,memberof | fl

# Filter group membership, including inhereted groups
Get-ADGroup -Filter 'member -RecursiveMatch "$USER_DN"' | select name
Get-ADGroup -LDAPFilter '(member:1.2.840.113556.1.4.1941:=$USER_DN)' | select Name

# Get an object in an unknown OU
Get-ADObject -Filter 'Name -like "$OBJECT"'

# Get users within an OU
Get-ADUser -Filter * -SearchBase '$OU_DN'
```

See [this](https://learn.microsoft.com/en-us/archive/technet-wiki/5392.active-directory-ldap-syntax-filters) guide for writing filters.

**Note:** `-Filter` can use single quotes, double quotes, or curly braces.
