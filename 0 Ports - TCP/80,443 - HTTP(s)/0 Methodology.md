# Methodology

## Enumerate

```powershell
# Add domain to /etc/hosts if necessary
echo "$IP example.com" | sudo tee -a /etc/hosts

# Fuzz vhosts
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://example.com -H 'Host: FUZZ.example.com'

# ENTIRELY enumerate the web page
firefox http://example.com &; disown

# If the site is using wordpress
wpscan --url http://example.com --detection-mode aggressive -e ap,u -o wpscan.out

# Check extensions
ffuf -w /usr/share/seclists/Discovery/Web-Content/web-extensions.txt -u http://example.com/indexFUZZ

# Briefly check source code (ignore css/js for now)
Ctrl + u

# Any abnormal clicks should be investigated manually with burp

# Imediately jump to any discovered vhosts, first adding them to /etc/hosts
firefox http://dev.example.com &; disown

# If the vhost is blocked or redirected (403, 302, etc.), Start fuzzing for endpoints
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -u http://dev.example.com/FUZZ
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-directories.txt -u http://dev.example.com/FUZZ
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-files.txt -u http://dev.example.com/FUZZ

# If any suspicious directories are discovered, fuzz them
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-files.txt -u http://dev.example.com/suspicious_dir/FUZZ
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-directories.txt -u http://dev.example.com/suspicious_dir/FUZZ

# Search for specific extensions
ffuf -w /usr/share/seclists/Discovery/Web-Content/raft-small-words.txt -u http://example.com/FUZZ -e .pdf,.txt

# If still no leads, try a bigger subdomain list
ffuf -w /usr/share/seclists/Discovery/DNS/subdomains-top1million-20000.txt -u http://example.com -H 'Host: FUZZ.example.com'

# If still no leads, start recursive scans, prioritizing discovered vhosts
feroxbuster -u http://dev.example.com
feroxbuster -u http://example.com
feroxbuster -u http://example.com -x php
```

**Special Characters String:**

```
# Special Characters
~!@#$%^&*()-_+={}][|\`,./?;:'"<>

# URL-encoded key characters
~!%40%23$%25^%26*()-_%2b%3d{}][|\`,./%3f%3b%3a'"<>

# URL-encoded all characters
%7e%21%40%23%24%25%5e%26%2a%28%29%2d%5f%2b%3d%7b%7d%5d%5b%7c%5c%60%2c%2e%2f%3f%3b%3a%27%22%3c%3e
```
