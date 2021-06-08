# Enumerating Network Services

## SMB
___
### NMap
- List SMB shares using NMap scripts
```sh
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 0.0.0.0
```

### SMBMap
- Enumerate SMB Share across a whole network
```sh
smbmap -H 0.0.0.0
```

### Smbclient
- Browse SMB share
```sh
smbclient //0.0.0.0/share
```

## NFS
___
### NMap
- List NFS using NMap scripts
```sh
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 0.0.0.0
```

## Links
___
### Guides
- https://www.hackingarticles.in/smb-penetration-testing-port-445/

### Writeups
- 