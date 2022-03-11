## nmap
- run all dns scripts
```bash
#Perform enumeration actions
nmap -n --script "(default and *dns*) or fcrdns or dns-srv-enum or dns-random-txid or dns-random-srcport" $ip
```