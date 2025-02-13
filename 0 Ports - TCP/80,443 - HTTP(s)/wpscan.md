---
tags:
  - tool
  - wordpress
---
# wpscan

Scan wordpress sites

## Capabilities

```powershell
# Basic scan
wpscan --url http://example.com --detection-mode aggressive -o wpscan.out
wpscan --url http://example.com --disable-tls-checks --detection-mode aggressive -o wpscan.out

# Check plugins/users
wpscan --url http://example.com --detection-mode aggressive -e ap,u -o wpscan.out
wpscan --url http://example.com --detection-mode aggressive -e ap,u --plugins-detection aggressive -o wpscan_long.out # Can take up to an hour
```

### API Token

In order to not have to supply your API token everytime you run the command, add the following to `~/.wpscan/scan.yml`:

```yml
cli_options:
  api_token: '$TOKEN'
```

View your API token [here](https://wpscan.com/profile/) (you will have to make an account if you dont already have one).
