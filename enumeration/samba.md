### impacket
- enumerate shares
```bash
crackmapexec smb $ip --shares -u user -p password
```
### nmap
- list shares
```sh
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 0.0.0.0
```

### enum4linux
- enumerate shares
```bash
enum4linux -a -u "guest" -p "" $ip
crackmapexec smb $ip --shares -u username -p password
```

### smbmap
- enumerate shares across a whole network
```sh
smbmap -H 0.0.0.0
```
- enumerate shares with guest access on single target
```bash
smbmap -u "guest" -p "" -P 445 -H $ip
smbmap -u username -p password -P 445 -H $ip
```

### smbclient
- browse share
```sh
smbclient //$ip/share
smbclient -U username //$ip/share
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