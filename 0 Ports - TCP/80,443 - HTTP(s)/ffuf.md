---
tags:
  - tool
  - web
  - fuzzing
---
# ffuf

Fuzz everything

## Capabilities

```powershell
# VHost fuzzing
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://example.com -H 'Host: FUZZ.example.com'

# Initial fuzz
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -u http://example.com/FUZZ

# File fuzzing
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-files.txt -u http://example.com/FUZZ

# Directory fuzzing (use directory-list-2.3-small.txt on the OSCP)
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-directories.txt -u http://example.com/FUZZ -recursion -recursion-depth 4
ffuf -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-small.txt -u http://example.com/FUZZ -ic -recursion -e .php,.aspx

# Extension fuzzing
ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt -u http://example.com/indexFUZZ

# GET parameter fuzzing
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://example.com?FUZZ=a

# GET parameter value fuzzing
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://example.com?<param>=FUZZ

# POST parameter fuzzing
ffuf -w /usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt -u http://example.com -X POST -d 'FUZZ=key' -H 'Content-Type: application/x-www-form-urlencoded'

# FUZZ using a request copied from burp (helpful with json data)
ffuf -request search.req -request-proto http -w /usr/share/seclists/Usernames/Names/names.txt

# Same as previous example but using 2 wordlists to fuzz 2 values inside of request
# The nums.txt wordlist is used first as it is extremely short and we want it to be fully exhausted before moving to the next name
ffuf -request search.req -request-proto http -w nums.txt:F2,/usr/share/seclists/Usernames/Names/names.txt:F1

# Same as previous example except we are using an inline sequence command instead of having to create a wordlist
# We use <() process substitution instead of $() command substitution to allow us to treat the output of the command as a file
ffuf -request search.req -request-proto http -w <(seq 0 7):F2,/usr/share/seclists/Usernames/Names/names.txt:F1

# Use burp proxy to intercept and view request before sending it
ffuf -w /usr/share/seclists/Fuzzing/special-chars.txt -u http://example.com?param=FUZZ -x http://localhost:8080

# Sub-domain fuzzing (public DNS records, bad practice)
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://FUZZ.example.com
```

**Note:**

- Use lowercase wordlists when fuzzing case-insensitive web servers (common with Windows)
- If initial directory scan yields nothing and you have no other leads, use a list for the specific technology you are encountering.

**My `/home/kali/.config/ffuf/ffufrc`**:

```
[general]
    colors = true
    threads = 50
```

## Wordlists

### General Purpose

```bash
# Vhosts/subdomains
/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt  # 4989
/usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt # 19966

# Extensions
/usr/share/seclists/Discovery/Web-Content/web-extensions.txt # 41

# Directory Fuzzing
/usr/share/seclists/Discovery/Web-Content/raft-small-directories.txt  # 20116
/usr/share/seclists/Discovery/Web-Content/raft-medium-directories.txt # 30000

# File Fuzzing
/usr/share/seclists/Discovery/Web-Content/raft-small-files.txt  # 11424
/usr/share/seclists/Discovery/Web-Content/raft-medium-files.txt # 17129

# Parameter names
/usr/share/seclists/Discovery/Web-Content/burp-parameter-names.txt # 6453

# Special characters
/usr/share/seclists/Fuzzing/special-chars.txt # 32

# All characters and symbols
/usr/share/seclists/Fuzzing/alphanum-case-extra.txt # 95

# Usernames
/home/kali/repos/statistically-likely-usernames/jsmith2.txt     # 4997
/usr/share/seclists/Usernames/Names/names.txt                   # 10177
/home/kali/repos/statistically-likely-usernames/jsmith.txt      # 48705
/usr/share/seclists/Usernames/xato-net-10-million-usernames.txt # 8295455

# SSH Key formats
id_rsa
id_ecdsa
id_ecdsa_sk
id_ed25519
id_ed25519_sk
```

**Note:** `directory-list-2.3` contains comments and requires `-ic` to ignore comments in `ffuf`

### Exploit Specific

```bash
# LFI
/usr/share/seclists/Fuzzing/LFI/LFI-Jhaddix.txt # 929


```