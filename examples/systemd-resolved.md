
## systemd-resolved

systemd-resolved is the new dns client for most linux distributions. sometimes the configuration can get a bit wonky if you've enforced the legacy dns client, but realy most of your problems could be resolved by ensuring the following configurations are in order:

### 1: resolv.conf
point resolv.conf to the systemd config:
```
:~$ sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.confi
```

### 2: nsswitch.conf
ensure the configuration of your hosts directive in nsswitch looks like this:
```
:~$ sudo vim /etc/nsswitch.conf
...
hosts:          files resolve mdns4_minimal [!UNAVAIL=return] dns myhostname
...
```

### 3: systemd-resolved unit
ensure systemd-resolved service is running and enabled:
```
:~$ sudo systemctl status systemd-resolved
:~$ sudo systemctl enable systemd-resovled
```

### 4: resolved.conf
add domain suffixes/sarch domains to resolved.conf if required:
```
:~$ sudo vim /etc/systemd/resolved.conf
...
Domains=mysearchdomain1.com mydomainsuffix2.net someotherdomain.com.au
...
```
turn off domain caching if you want to be a horrible person towards your resolving name servers or do something terrible with round-robin DNS:
```
Cache=No
```


### 5: apply the changes
restart all the things, including NetworkManager:
```
:~$ sudo systemctl restart resolvconf systemd-resolved NetworkManager
```

### 6: test changes
check the systemd-resolve status and name resolution:
```
:~$ systemd-resolve --status
:~$ dig testdomain.net
```

### 7: clear dns client cache
to clear the dns resolution cache, just restart the systemd-resolved unit:
```
:~$ systemctl restart systemd-resovled
```
