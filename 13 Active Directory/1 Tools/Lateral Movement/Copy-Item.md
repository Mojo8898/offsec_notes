---
tags:
  - tool
---
# Copy-Item

Move files between Windows systems stealthily.

## Capabilities

```powershell
# Start a new PS session
$sessionSRV02 = New-PSSession -ComputerName $HOST -Credential $credential

# Copy file from current machine to target
Copy-Item -ToSession $sessionSRV02 -Path '$LOCAL_FILE' -Destination '$REMOTE_FILE' -Verbose

# Copy file from target to current machine
Copy-Item -FromSession $sessionSRV02 -Path '$REMOTE_FILE' -Destination '$LOCAL_FILE' -Verbose
```
