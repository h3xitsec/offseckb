# nmap
- List NFS using NMap scripts
```sh
nmap -p 445 --script=smb-enum-shares.nse,smb-enum-users.nse 0.0.0.0
```
