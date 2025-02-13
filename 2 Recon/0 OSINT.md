# OSINT

### Google

```powershell
# Hosted pdf's
filetype:pdf inurl:$DOMAIN

# Email addresses
intext:"@$DOMAIN" inurl:$DOMAIN
```

## DNS

### DNSDumpster

[DNSDumpster](https://dnsdumpster.com/) is a free domain research tool that can discover hosts related to a domain.

### ViewDNS

[ViewDNS](https://viewdns.info/) is a swiss army knife for DNS.

## whois

Directly query domain information.

```powershell
# Query the domain `megacorpone.com` using the WHOIS server `192.168.50.251`
whois example.com -h $IP
```

## Subdomains

### ctr.sh

[crt.sh](https://crt.sh/) is a web interface to a distributed database called the certificate transparency logs.

### Knockpy

[Knockpy](https://github.com/guelfoweb/knock) is a portable and modular `python3` tool designed to quickly enumerate subdomains on a target domain through _passive reconnaissance_ and _dictionary scan_.

## ASN

### Hurricane Electric

[Hurricane Electric](https://bgp.he.net) is a resource for researching what address blocks are assigned to an organization and what ASN they reside within.

## Usernames

### linkedin2username

[linkedin2username](https://github.com/initstring/linkedin2username) is a tool for generating username lists from companies on LinkedIn.

### statistically-likely-usernames

[statistically-likely-usernames](https://github.com/insidetrust/statistically-likely-usernames) is a resource containing wordlists for creating statistically likely username lists for use in password attacks and security testing.

## Credentials

### trufflehog

[trufflehog](https://github.com/trufflesecurity/trufflehog) is a tool for finding, verifying, and analyzing leaked credentials.

```powershell
# Scan a github repo for only verified secrets
trufflehog git https://github.com/trufflesecurity/test_keys --only-verified

# Scan a github repo + its issues and pull requests
trufflehog github --repo https://github.com/trufflesecurity/test_keys --issue-comments --pr-comments
```

**Note:** The output is generally separated between the TLD (`.com`) and the specific domain (`megacorpone.com`)

## Dehashed (paid)

Hunt for cleartext credentials and password hashes in breach data.

- https://dehashed.com/
- https://github.com/sm00v/Dehashed
