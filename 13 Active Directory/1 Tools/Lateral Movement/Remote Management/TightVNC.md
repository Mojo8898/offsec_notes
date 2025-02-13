---
tags:
  - tool
---
# TightVNC

Connect remotely with a full graphical interface to any OS.

## Local

```powershell
# Check stored credentials
reg query HKLM\SOFTWARE\TightVNC\Server /s

# Convert passsword to plaintext (on kali)
echo -n $PASSWD | xxd -r -p | openssl enc -des-cbc --nopad --nosalt -K $KEY -iv 0000000000000000 -d | hexdump -Cv
```

**Note:** We can use the repository [PasswordDecrypts](https://github.com/frizb/PasswordDecrypts) to search for the registry keys that common VNC software uses.

## Remote

```powershell
# Install xtightvncviewer
sudo apt install xtightvncviewer

# Authenticate
echo $PASSWD | vncviewer $IP -autopass

# Better auth for unstable connection
echo $PASSWD | vncviewer $IP -autopass -quality 0 -nojpeg -compresslevel 1 -encodings "tight hextile" -bgr233
```

We can then use `F8` to interact with the remote machine, which will give us a prompt with different options.
