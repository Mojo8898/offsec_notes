# Joomla

[HackTricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-web/joomla)

## Enumeration

See [joomscan](https://github.com/OWASP/joomscan)

```powershell
joomscan -u http://$IP
```

Joomla! < 4.2.8 - Unauthenticated information disclosure: [CVE-2023-23752 POC](https://github.com/Acceis/exploit-CVE-2023-23752)
