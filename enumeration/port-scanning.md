# Port Scanning

## NMap

- Port range scanning
```
sudo nmap -A -sS -p 1-1000 0.0.0.0
```

- Port range scanning with vulnerability enumeration
```sh
sudo nmap -sV -sC --script vuln -oN out.nmap 0.0.0.0
```

- Quick range scanning with rustscan
  
```
rustscan -a 0.0.0.0 -r 1-65535 --ulimit 5000
```
- Optimized port scanning with NMap scripts
  
```
nmapAutomator.sh -H 0.0.0.0 -t script
```