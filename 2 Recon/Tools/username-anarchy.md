---
tags:
  - tool
---
# username-anarchy

[username-anarchy](https://github.com/urbanadventurer/username-anarchy) is a tool for generating usernames when penetration testing.

## Capabilities

**names.txt**

```
john smith
<SNIP>
```

### Generate Wordlist

```powershell
# Generate userlist with all formats
username-anarchy -i names.txt > test_users.txt

# Generate userlist with specified formats
username-anarchy -i names.txt -f f.last > users.txt
```
