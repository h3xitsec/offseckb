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

## wfuzz
`wfuzz -c -z file,/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://domain.tld -H "Host: FUZZ.domain.tld"`
- --hw to hide results based on words count in response
- --hc to hide results based response code
- --hl to hide results based on line count in response

## ffuf
`ffuf -c -u http://domain.tld -H "Host: FUZZ.domain.tld" -w /opt/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt`

- Filter options
	- -fc : status code
	- -fl : nb of lines
	- -fr : regex
	- -fs : reponse size
	- -fw : nb of words