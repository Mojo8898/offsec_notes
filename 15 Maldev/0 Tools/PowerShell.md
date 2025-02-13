# PowerShell

## Defender

```powershell
# Check defender commands
Get-Command -Module Defender

# Check status details about antimalware software
Get-MpComputerStatus

# View detected threat history
Get-MpThreat

# View the threat detection history
Get-MpThreatDetection -ThreatID $ID

# Check real-time protection status
Get-MpPreference

# enable/disable real-time protection
Set-MpPreference -DisableRealTimeMonitoring $true
```
