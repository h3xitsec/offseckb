
### nmap
- list shares
```sh
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 0.0.0.0
```

### enum4linux
- enumerate shares with guest access
```bash
enum4linux -a -u "guest" -p "" $ip
```

### smbmap
- enumerate shares across a whole network
```sh
smbmap -H 0.0.0.0
```
- enumerate shares with guest access on single target
```bash
smbmap -u "guest" -p "" -P 445 -H $ip
```

### smbclient
- browse share
```sh
smbclient //0.0.0.0/share
```
- recursively download folder
```
mask ""
recurse ON
prompt OFF
mget *
```

### Links
___
#### Guides
- https://www.hackingarticles.in/smb-penetration-testing-port-445/

#### Writeups
- 