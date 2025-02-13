# Methodology

## Enumerate

```powershell
# Start with a banner grab
nc -vn $IP 25

# Check for open relay (no auth required)
nmap -p 25 --script smtp-open-relay $IP -v

# Send an email with our IP to see if they click on it
swaks -t $TO_USER@$TO_DOMAIN -f $FROM_USER@$FROM_DOMAIN -h 'Subject: IMPORTANT' --body 'http://$OUR_IP' -s $IP --suppress-data
```

## Connect

```powershell
telnet $IP 25
```

### Session Commands

See [HackTricks](https://book.hacktricks.xyz/network-services-pentesting/pentesting-smtp/smtp-commands)
