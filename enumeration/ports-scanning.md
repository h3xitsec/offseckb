# Ports Scanning
## nmap
- host discovery
```bash
nmap -sL -n 10.10.0-255.101-125
```
- arp host discovery (must be on same lan)
```bash
sudo nmap -PR -sn 10.0.0.0/24
```
- icmp address mask host discovery
```bash
sudo nmap -PM -sn 10.0.0.0/24
```
- icmp timestamp host discovery
```bash
sudo nmap -PP -sn 10.0.0.0/24
```
- icmp echo host discovery
```bash
sudo nmap -PE -sn 10.0.0.0/24
```
- tcp syn ping host discovery
```bash
sudo nmap -PS[80,81-82] -sn 10.0.0.0/24
```
- tcp syn ack host discovery
```bash
sudo nmap -PA[80,81-82] -sn 10.0.0.0/24
```
- udp ping host discovery
```bash
sudo nmap -PU -sn 10.0.0.0/24
```
- port range scan with vulnerability enumeration
```sh
sudo nmap -sV -sC --script vuln -oN out.nmap 0.0.0.0
```
- dhcp discovery
```bash
sudo nmap --script broadcast-dhcp-discover
```

## nmapAutomator
- Fast port scan
```bash
nmapAutomator.sh -H $ip -t port
```
- Port scan + nmap scripts
```bash
nmapAutomator.sh -H 0.0.0.0 -t script
```

## nc
- Port range scanning
```bash
nc -zv 127.0.0.1 1-65535
```

## rustscan
- scan all ports
```bash
rustscan -a 0.0.0.0 -r 1-65535 --ulimit 5000
```
- scan all ports + os/service discovery and scripts by nmap
```bash
rustscan -a 0.0.0.0 -r 1-65535 --ulimit 5000 -- -A -sC
```