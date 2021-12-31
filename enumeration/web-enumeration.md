# dirsearch
- basic content discovery
```bash
dirsearch -u http://domain.tld
```
- content discovery with files extension tests
```bash
dirsearch -e conf,config,bak,backup,swp,old,db,sql,asp,aspx,aspx~,asp~,py,py~,rb,rb~,php,php~,bak,bkp,cache,cgi,conf,csv,html,inc,jar,js,json,jsp,jsp~,lock,log,rar,old,sql,sql.gz,sql.zip,sql.tar.gz,sql~,swp,swp~,tar,tar.bz2,tar.gz,txt,wadl,zip,log,xml,js,json -u http://domain.com
```

# gobuster
- basic content discovery
```bash
gobuster dir -f -u http://host.dom -w /opt/wordlists/seclists/Discovery/Web-Content/common.txt
```
- wildcard mode with ignore based on size of response
```bash
gobuster dir -f -u http://hostname.domain -w /opt/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt --wildcard --exclude-length 111111
```
- content discovery with files extension tests
```bash
gobuster dir -u http://www.domain.tld -t 100 -e -x php,html,js,txt -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt
```
## wfuzz
- basic content discovery
```bash
wfuzz -c -z file,/opt/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt -u http://hostname.tld/api/FUZZ --hw 12
```

## ffuf
- basic content discovery
```bash
ffuf -u http://hostname.tld/FUZZ -w /opt/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```
- find valid users
```bash
ffuf -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -u http://MACHINE_IP/customers/signup -mr "username already exists"
```
- Filter options
	- -fc : status code
	- -fl : nb of lines
	- -fr : regex
	- -fs : reponse size
	- -fw : nb of words