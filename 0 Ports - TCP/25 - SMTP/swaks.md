---
tags:
  - tool
  - mail
---
# swaks

All things SMTP

## Capabilities

```powershell
# Get help menu
swaks --help

# Send an email
swaks -t $TO_USER@$TO_DOMAIN -f $FROM_USER@$FROM_DOMAIN -h 'Subject: IMPORTANT' --body '$BODY'
swaks -t $TO_USER@$TO_DOMAIN -f $FROM_USER@$FROM_DOMAIN -h 'Subject: IMPORTANT' --body '$BODY' -s $IP
swaks -t $TO_USER@$TO_DOMAIN -f $FROM_USER@$FROM_DOMAIN -h 'Subject: IMPORTANT' --body '$BODY' -s $IP --suppress-data

# Send an email with an attachment
swaks -t $TO_USER@$TO_DOMAIN -f $FROM_USER@$FROM_DOMAIN --attach @example.txt -h 'Subject: IMPORTANT' --body '$BODY' -s $IP

# Add authentication
swaks -t $TO_USER@$TO_DOMAIN -f $FROM_USER@$FROM_DOMAIN -au user@example.com -ap 'password' -h 'Subject: IMPORTANT' --body '$BODY' -s $IP

# Add TO_DOMAIN if error
swaks -t $TO_USER@$TO_DOMAIN -f $FROM_USER@$FROM_DOMAIN -au user@example.com -ap 'password' -h 'Subject: IMPORTANT' --body '$BODY' -s $IP --ehlo $TO_DOMAIN
```
