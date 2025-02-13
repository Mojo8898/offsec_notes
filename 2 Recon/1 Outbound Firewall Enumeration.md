# Outbound Firewall Enumeration

## From Linux

We can manually enumerate outbound firewall rules with nc using the following commands.

```powershell
nc -zvw 1 $OUR_IP $PORT
```

The following response means the port is available for us to use for pivoting or listening if the target is not listening on that port already.

```
nc: connect to ... port ... (tcp) failed: Connection refused
```

The following response on the other hand means a firewall rule is blocking the outbound connection.

```
nc: connect to ... port ... (tcp) timed out: Operation now in progress
```

## Misc

Commonly available ports that are not necessarily listening:

```
53
80
123
137
138
139
443
445
```

For example, we can set the following listeners when 80 and 123 are allowed outbound.

```
[Agent : riley@mail] » listener_add --addr 0.0.0.0:8443 --to 127.0.0.1:80
INFO[0532] Listener 3 created on remote agent!          
[Agent : riley@mail] » listener_add --addr 0.0.0.0:8444 --to 127.0.0.1:123
INFO[0547] Listener 4 created on remote agent!
```

We can then test the inbound rules to ensure proper communication with the following commands on our host.

```
nc -lvnp 80
```
