---
tags:
  - tool
  - rdp
---
# xfreerdp

Connect to Windows machines via RDP

## Capabilities

```powershell
# Basic connect
xfreerdp /cert-ignore /u:$USER /p:'$PASSWD' /v:$IP /dynamic-resolution

# QOL options
xfreerdp /cert-ignore /d:$DOMAIN /u:$USER /p:'$PASSWD' /v:$IP /dynamic-resolution /auto-reconnect /clipboard /compression /bpp:8 /audio-mode:0 -window-drag -themes -wallpaper

# Pass the hash (requires Restricted Admin Mode)
xfreerdp /cert-ignore /d:$DOMAIN /u:$USER /pth:$NTHASH /v:$IP /dynamic-resolution /auto-reconnect /clipboard /compression /bpp:8 /audio-mode:0 -window-drag -themes -wallpaper
```

**Note:** `/drive:.,linux` shares the current working directory on your attack box, discoverable via `This PC` in `File Explorer`. Including a share like this can break the connection.
