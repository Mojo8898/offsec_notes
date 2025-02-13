---
tags:
  - tool
---
# PowerUpSQL

PowerView for MSSQL

## Capabilities

Cheatsheet: https://github.com/NetSPI/PowerUpSQL/wiki/PowerUpSQL-Cheat-Sheet

```powershell
# Import module
Import-Module .\PowerUpSQL.ps1

# Check for SQL server instances
Get-SQLInstanceDomain

# Authenticate and execute custom query
Get-SQLQuery -Verbose -Instance "$IP,1433" -username "$DOMAIN\$USER" -password "$PASS" -query 'Select @@version'
```
