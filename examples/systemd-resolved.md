
## systemd-resolved

if you've upgraded from older version of linux pre-systemd-resolved, you may be faced with occasional problems with DNS resolution or have bypass systemd-resolved because it was causing you problems. you may also wonder how do i add search domains when using systemd-resolved.

you can simply resolve these issue by ensuring the following configurations are in order:

### 1: point resovl.conf to the systemd config:
```:~$ sudo ln -sf /run/systemd/resolve/resolv.conf /etc/resolv.conf```

### 2: nsswitch.conf:
ensure the configuration of your hosts directive in nsswitch looks like this:
```
:~$ sudo vim /etc/nsswitch.conf
...
hosts:          files resolve mdns4_minimal [!UNAVAIL=return] dns myhostname
...
```

### 3: ensure systemd-resolved service is running and enabled:
```
:~$ sudo systemctl status systemd-resolved
:~$ sudo systemctl enable systemd-resovled
```

### 4: add domain suffixes/search domains to resolved.conf:
```
:~$ sudo vim /etc/systemd/resolved.conf
...
Domains=mysearchdomain1.com mydomainsuffix2.net someotherdomain.com.au
...
```

### 5: restart all the things, including NetworkManager:
```sudo systemctl restart resolvconf systemd-resolved NetworkManager```

### 6: check the systemd-resolve status and name resolution:
```:~$ systemd-resolve --status
:~$ dig testdomain.net```
