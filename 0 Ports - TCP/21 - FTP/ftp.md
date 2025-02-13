---
tags:
  - tool
  - ftp
---
# ftp

Connect via FTP

## Capabilities

```powershell
# Connect, anonymous authentication - anonymous:a
ftp $IP
ftp -A $IP

# Browser connection
firefox ftp://$USER:$PASSWD@$IP

# Download files
wget -m ftp://$USER:$PASSWD@$IP
wget -m --no-passive ftp://$USER:$PASSWD@$IP
```

### Session Commands

```bash
# Swap between active and passive mode, useful when you receive the response: "229 Entering Extended Passive Mode (|||60561|)"
passive

# Swap between binary and ASCII (binary required when uploading binaries)
binary
ascii

# List all files
dir

# Download file(s)
get example.txt
mget *.txt

# Upload file(s)
put example.txt
mput *.txt

# Delete file(s)
delete example.txt
mdelete *.txt

# Make directories
mkdir example_dir

# Navigate filesystem
pwd
cd example_dir

# Open shell
!
```
