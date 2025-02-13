---
tags:
---
# pwncat

[pwncat](https://github.com/calebstewart/pwncat) is a fancy reverse and bind shell handler.

## Capabilities

```powershell
# Initialize environment
pwncat-cs

# Reverse shell
pwncat-cs -lp 9001

# Bind shell
pwncat-cs 192.168.1.1 9001

# List sessions
sessions

# Windows listener
listen --platform windows 9001
sessions 0

# Upload file (local mode)
upload linpeas.sh
```

**Notes:**

- Drop into a session with `Ctrl + d` (toggle between `local` and `remote` mode)
- Prefix a command with the word `local` inside a shell to run commands locally
- If you need to send `Ctrl + d` or `Ctrl + k` to the target, prefix with `Ctrl + k`
