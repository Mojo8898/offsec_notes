---
tags:
  - tool
---
# dsacls

[dsacls](https://learn.microsoft.com/en-us/previous-versions/windows/it-pro/windows-server-2012-r2-and-2012/cc771151(v=ws.11)) (the command-line equivalent to the `Security` tab in the `Properties` dialog box of `ADUC`) is a native Windows binary that can display and change `ACEs`/`permissions` in `ACLs` of AD objects.

## Capabilities

```powershell
# Read DACL
dsacls "$OBJECT"

# Read permissions that principal has over target object
dsacls "$OBJECT_DN" | Select-String "$PRINCIPAL"
```
