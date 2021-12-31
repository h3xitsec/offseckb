# nmap
- List SMB shares using NMap scripts
```sh
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 0.0.0.0
```

# smbmap
- Enumerate SMB Share across a whole network
```sh
smbmap -H 0.0.0.0
```

# smbclient
- Browse SMB share
```sh
smbclient //0.0.0.0/share
```

- Recursively download folder (in smbclient)
```
mask ""
recurse ON
prompt OFF
mget *
```

# Links
___
### Guides
- https://www.hackingarticles.in/smb-penetration-testing-port-445/

### Writeups
- 