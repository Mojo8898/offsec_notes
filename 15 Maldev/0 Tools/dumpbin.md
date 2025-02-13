---
tags:
  - tool
---
# dumpbin

Analyze PE files.

## Capabilities

```powershell
# Print metadata in PE file
dumpbin /headers $PE_FILE

# View functions imported by PE file
dumpbin /imports $PE_FILE

# View functions exported by DLL
dumpbin /exports $DLL_FILE
```
