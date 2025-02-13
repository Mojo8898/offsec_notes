---
tags:
  - tool
---
# bloodhound-python

Run bloodhound remotely on a target.

## Capabilities

```powershell
# Run all checks against the target
bloodhound-python -d $DOMAIN -c All -u $USER -p '$PASSWD' -ns $IP -k

# Zip newly created files
zip -r $FILE.zip *.json
```

**Note:** Kerberos authentication requires the host to resolve the domain FQDN. This means that the option `--nameserver` is not enough for Kerberos authentication because our host needs to resolve the DNS name KDC for Kerberos to work. If we want to use Kerberos authentication, we need to set the DNS server to the target machine or configure the DNS entry in our `/etc/hosts` file.
