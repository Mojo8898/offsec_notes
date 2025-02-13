---
tags:
  - tool
  - winrm
---
# evil-winrm

Authenticate with WinRM

## Capabilities

```powershell
# Connect with credentials
evil-winrm -i $IP -u $USER -p '$PASS'
evil-winrm -i $IP -u '$DOMAIN\$USER' -p '$PASS'

# Connect using SSL
evil-winrm -i $IP -S -c <cert> -k <priv-key>

# Specify powershell scripts to import
evil-winrm -i $IP -u $USER -p '$PASS' -s '/home/kali/staging/'
```

### Session Commands

```powershell
# Display help menu
menu

# Upload file
upload $FILE

# Download file
download $FILE

# Use scripts
Bypass-4MSI
$SCRIPT
```

### Kerberos Authentication

To authenticate with certain kerberos environments, the FQDN needs to be specified and the `/etc/krb5.conf` file needs to be modified to the following for the example FQDN `dc01.offsec.local`.

```powershell
[libdefaults]
    default_realm = OFFSEC.LOCAL

# The following krb5.conf variables are only for MIT Kerberos.
    kdc_timesync = 1
    ccache_type = 4
    forwardable = true
    proxiable = true
    rdns = false

# The following libdefaults parameters are only for Heimdal Kerberos.
    fcc-mit-ticketflags = true

[realms]
    OFFSEC.LOCAL = {
        kdc = dc01.offsec.local
        admin_server = dc01.offsec.local
    }

[domain_realm]
    .offsec.local = OFFSEC.LOCAL
    offsec.local = OFFSEC.LOCAL
```

All `offsec.local` or `dc01.offsec.local` (case insensitive) values need to be modified accordingly.

```bash
# Connect via kerberos after modifying /etc/krb5.conf
evil-winrm -i dc01.offsec.local -r offsec.local
```
