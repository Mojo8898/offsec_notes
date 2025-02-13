---
tags:
  - tool
  - ssh
  - routing
---
# ssh

Secure Shell

## Capabilities

### Remote Fowarding

```powershell
# Connect to our attack box, listen on port 2345, and forward all traffic to 10.0.0.10:5432
ssh -N -R 127.0.0.1:2345:10.0.0.10:5432 kali@$OUR_IP

# Connect to our attack box, and act as a SOCKS proxy operating on port 1080
ssh -N -R 1080 kali@$OUR_IP
```

### Local Fowarding

```powershell
# Connect to 10.0.0.10 and listen on 4455, forwarding to 172.16.20.1:445
ssh -N -L 0.0.0.0:4455:172.16.20.1:445 $USER@$IP

# Connect to 10.0.0.10 and listen on 9999, forwarding all traffic (simulating a SOCKS proxy)
ssh -N -D 0.0.0.0:9999 $USER@$IP
```

**Note:**

- `-N` means no shell
- `-L` think local port forwarding
- `-D` think dynamic port forwarding (SOCKS proxy)
- `-R` think remote port forwarding

Lets break down the last example with substituted values, and say our IP is `192.168.1.10`:

```powershell
ssh -N -D 0.0.0.0:9999 sql_admin@10.0.0.10
```

We are running the `ssh` command on a reverse shell we have already established to `10.0.0.10`. Upon executing the command, `10.0.0.10` essentially becomes a SOCKS proxy for us to send traffic to that will then be forwarded to `10.0.0.10`, and `10.0.0.10` will forward the traffic to its final destination.

We can then use [proxychains](../../20%20Routing/proxychains.md) to interact with `10.0.0.10` by adding the following to our `/etc/proxychains4.conf`

```powershell
# ... At the very bottom of the file, we set the ProxyList to only point to port 9999.
[ProxyList]
socks5 10.0.0.10 9999
```

Now in the following command

```powershell
proxychains -q smbclient -L //172.16.20.1/ -U $USER --password=$PASSWD
```

The request will first be taken by `proxychains` and fowarded to `192.168.50.63:9999` as specified in our `/etc/proxychains4.conf`. The traffic is then tunneled to `10.0.0.10:9999`, where `10.0.0.10` then fowards it to its final destination of `172.16.20.1:445`.
