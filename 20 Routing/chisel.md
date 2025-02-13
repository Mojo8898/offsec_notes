---
tags:
  - tool
  - routing
---
# chisel

Route network traffic in restricted environments

## Capabilities

```powershell
# Listen on 8050
chisel server -p 8050 --reverse

# Listen on 8050, allowing chisel to see the local socks5 proxy (proxychains)
chisel server --socks5 -p 8050 --reverse
```

REDACTED THEORY
