# DACL Enumeration

## UNIX-like

```powershell
# Read DACL
dacledit.py -target $PRINCIPAL $DOMAIN/$USER:'$PASSWD'

# Read permissions that principal has over target object
dacledit.py -principal $PRINCIPAL -target '$TARGET_OBJ' $DOMAIN/$USER:'$PASSWD'

# Using nxc
nxc ldap $IP -u $USER -p '$PASSWD' -M daclread -o PRINCIPAL=$PRINCIPAL TARGET=$TARGET_OBJECT
```

## Windows

```powershell
# Read DACL
dsacls '$OBJECT_DN'

# Read permissions that principal has over target object
dsacls '$OBJECT_DN' | Select-String '$PRINCIPAL' -Context 0,5

# Using powerview
Get-ObjectAcl -ResolveGUIDs -Identity $TARGET_OBJ | select @{Name='SecurityIdentifier';Expression={Convert-SIDToName -SID $_.SecurityIdentifier}},ActiveDirectoryRights,ObjectAceType | sort -Property SecurityIdentifier
```
