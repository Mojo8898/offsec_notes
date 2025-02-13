# Methodology

## Connect

```powershell
# Connect with credentials
evil-winrm -u $USER -p '$PASSWD' -i $IP -s $SCRIPT_DIR

# Connect using SSL
evil-winrm -S -c $CERT_FILE -k $PRIV_KEY -i $IP
```

**Useful session commands:**

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

## Fundamentals

```powershell
# Check configuration
(get-item WSMan:\localhost\Client\TrustedHosts).value
```
