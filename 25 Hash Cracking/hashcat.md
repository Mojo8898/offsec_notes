---
tags:
  - tool
  - hash_cracking
---
# hashcat

Crack hashes

Reference: [example_hashes](https://hashcat.net/wiki/doku.php?id=example_hashes), make sure to remove escape characters from the hash if necessary (`\`)

## Capabilities

```bash
# Crack hash
hashcat -m 0 $HASH /usr/share/wordlists/rockyou.txt --force
hashcat -m 0 hashes/$HASH wordlists/rockyou.txt --force

# Crack hash with rule file
hashcat -m 0 $HASH /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/best64.rule --force
hashcat -m 0 $HASH /usr/share/wordlists/rockyou.txt -r /usr/share/hashcat/rules/rockyou-30000.rule --force

# Check cracked hash, add --show to the end of the last command
hashcat -m 0 $HASH /usr/share/wordlists/rockyou.txt --force --show

# Use a character-set
hashcat -a 3 -m 0 $HASH example_prepend_?d?d?d?d?d?d?d?d?d

# Benchmark
hashcat -b
```

**Notes:**

- Convert a variety of encryption types with `ENC2john` where `ENC` is some type of encryption such as `pfx`, `ssh`, `keepass` and more
- In the case a converted hash is still erroring after troubleshooting, resort to using 
