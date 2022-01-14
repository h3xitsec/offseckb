# Subdomains enumeration
## automation
### 1
```bash
gau --subs domain.com | unfurl domains >> subs1.txt
waybackurls domain.com | unfurl domains >> subs2.txt
subfinder -d domain.com -silent >> subs3.txt
cat subs1.txt subs2.txt subs3.txt | anew >> unique_subs.txt
```
## amass
`amass enun -d domain.tld`