---
tags:
  - tool
---
# wmic

WMIC is a command-line interface that allows administrators to query and manage various aspects of the Windows operating system programmatically.

## Windows Capabilities

### Enumeration

```powershell
# Get OS details
wmic /node:$IP os get Caption,CSDVersion,OSArchitecture,Version
Get-WmiObject -Class Win32_OperatingSystem -ComputerName $IP | Select-Object Caption, CSDVersion, OSArchitecture, Version

# Print patch details
wmic qfe get Caption,Description,HotFixID,InstalledOn

# Display basic host information
wmic computersystem get Name,Domain,Manufacturer,Model,Username,Roles /format:List

# List all processes
wmic process list /format:list

# Display domain information and domain controllers
wmic ntdomain list /format:list
wmic ntdomain get Caption,Description,DnsForestName,DomainName,DomainControllerAddress

# Display all local accounts and any domain accounts that have logged in
wmic useraccount list /format:list

# Display local groups
wmic group list /format:list

# Dump information about any system accounts being used as service accounts
wmic sysaccount list /format:list
```

Cheatsheet - https://gist.github.com/xorrior/67ee741af08cb1fc86511047550cdaf4

### Exploitation

```powershell
# Create a new process on remote machine
wmic /node:$IP process call create "notepad.exe"
Invoke-WmiMethod -Class Win32_Process -Name Create -ArgumentList "notepad.exe" -ComputerName $IP

# Include credentials
wmic /user:$USER /password:$PASSWD /node:$IP os get Caption,CSDVersion,OSArchitecture,Version
$cred = New-Object System.Management.Automation.PSCredential("$USER", (ConvertTo-SecureString "$PASSWD" -AsPlainText -Force));
Invoke-WmiMethod -Class Win32_Process -Name Create -ArgumentList "notepad.exe" -ComputerName $IP -Credential $cred
```

## Linux Capabilities

```powershell
# Install
sudo apt install wmi-client

# Execute commands
wmic -U $FQDN/$USER%$PASSWD //$IP "SELECT Caption, CSDVersion, OSArchitecture, Version FROM Win32_OperatingSystem"
```