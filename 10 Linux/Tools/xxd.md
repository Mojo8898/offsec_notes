---
tags:
  - tool
---
# xxd

Make a hexdump or do the reverse.

## Capabilities

```powershell
# Generate hexdump from binary
xxd $FILE

# Convert hexdump to file
echo '$HEX' | xxd -r -p > $OUTFILE
```
