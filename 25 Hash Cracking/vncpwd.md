---
tags:
  - tool
---
# vncpwd

Decrypt stored vnc passwords: https://github.com/jeroennijhof/vncpwd

## Capabilities

```bash
echo '6bcf2a4b6e5aca0f' | xxd -r -p > vnc.enc
vncpwd vnc.enc
```
