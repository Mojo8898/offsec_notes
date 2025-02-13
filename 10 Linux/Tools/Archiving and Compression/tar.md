---
tags:
  - tool
---
# tar

Decompress/export `tar` and `tar.gz` files.

## Capabilities

```powershell
# Extract tar.gz file
tar zxvf $FILE.tar.gz

# Compress directory to tar.gz (takes relative path)
tar zcvf $FILE.tar.gz --directory=$DIR .
```
