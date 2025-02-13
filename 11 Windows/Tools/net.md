---
tags:
  - tool
---
# net

Perform a variety of tasks within windows

## Capabilities

**Note:** The `/domain` option executes the command in the context of the domain.

```powershell
# Null session
net use \\$COMPUTER\ipc$ "" /u:""

# Guest account
net use \\DC01\ipc$ "" /u:guest

# Check password policy
net accounts
net accounts /domain

# Check domain information
net group /domain
net group "Domain Admins" /domain
net group "domain computers" /domain
net group "Domain Controllers" /domain
net group <domain_group_name> /domain

# List domain groups
net groups /domain

# List all local groups
net localgroup

# List domain users and groups that belong to the local administrators group
net localgroup administrators /domain

# Information about a group (admins)
net localgroup administrators

# Add user to administrators
net localgroup administrators <account_name> /add

# Check current shares
net share

# Get infromation about a user within the domain
net user <account_name> /domain

# List all users of the domain
net user /domain

# List information about the current user
net user %username%

# Mount a share locally
net use x: \computer\share

# Get a list of computers
net view

# Shares on the domains
net view /all /domain[:domainname]

# List shares of a computer
net view \computer /ALL

# List PCs of the domain
net view /domain
```
