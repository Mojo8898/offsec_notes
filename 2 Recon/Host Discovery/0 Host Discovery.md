# Host Discovery

```powershell
# Debug connection
traceroute -I $IP

# Passive ARP (local only)
sudo netdiscover -i eth0 -p

# Active ARP (local only)
sudo netdiscover -i eth0 -r $CIDR

# Scan with NetBIOS
nbtscan -r $CIDR

# Ping sweep
fping -asgq $CIDR

# Stealthier ping sweep (takes ~6 minutes)
fping -asgq -i 1358 $CIDR

# Only ping + port scan (bypasses certain firewalls)
sudo nmap -PO1 -n --top-ports 500 --open --min-rate 1000 $CIDR

# Targeting non-pinging network
sudo nmap -Pn -n --top-ports 500 --open --min-rate 1000 $CIDR

# Active local/remote Against windows targets
nxc smb $BASE_CIDR --generate-hosts-file hosts.txt
sudo tee -a /etc/hosts < hosts.txt

# Query computers and IPs with valid credentials
GetADComputers.py -resolveIP -dc-ip $DC_IP $DOMAIN/$USER:'$PASSWD'
```

**Note:** For quick TCP scans, top 500 ports is chosen as it is the lowest value that catches 5985.
