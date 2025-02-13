# Methodology

## Enumerate

```powershell
# Gather domain information
rpcinfo $DOMAIN
```

## Connect

```powershell
# Attempt null authentication
rpcclient $IP -U ''

# Attempt guest authentication
rpcclient $IP
```

### Session Commands

```powershell
# Enumerate users
enumdomusers
```
