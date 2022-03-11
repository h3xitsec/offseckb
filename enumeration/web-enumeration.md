## oneliners
- find open redirect vulnerabilities
```
echo "https://www.domain.com/" | waybackurls | httpx -silent -threads 100 | gf redirect | anew vuln.txt
```
## ffuf
- basic content discovery
```bash
ffuf -c -u $target/FUZZ -w /opt/wordlists/seclists/Discovery/Web-Content/directory-list-2.3-medium.txt
```
- extensions search
```bash
ffuf -c -u $target/FUZZ -e .conf,.config,.bak,.backup,.swp,.old,.db,.sql,.asp,.aspx,.aspx~,.asp~,.py,.py~,.rb,.rb~,.php,.php~,.bak,.bkp,.cache,.cgi,.conf,.csv,.html,.inc,.jar,.js,.json,.jsp,.jsp~,.lock,.log,.rar,.old,.sql,.sql.gz,.sql.zip,.sql.tar.gz,.sql~,.swp,.swp~,.tar,.tar.bz2,.tar.gz,.txt,.wadl,.zip,.log,.xml,.js,.json,.jpg,.jpeg,.png,.gif,.bmp -w /opt/wordlists/seclists/Discovery/Web-Content/raft-small-directories.txt -r
```
- multiple payload
```bash
ffuf -c -u $target/FUZZ1FUZZ2 -w /opt/wordlists/seclists/Discovery/Web-Content/raft-small-directories.txt:FUZZ1 -w /opt/wordlists/seclists/Discovery/Web-Content/raft-small-extensions.txt:FUZZ2 -r -v
```
- extensive content discovery
```bash
ffuf -c -u $target/FUZZ -e .conf,.config,.bak,.backup,.swp,.old,.db,.sql,.asp,.aspx,.aspx~,.asp~,.py,.py~,.rb,.rb~,.php,.php~,.bak,.bkp,.cache,.cgi,.conf,.csv,.html,.inc,.jar,.js,.json,.jsp,.jsp~,.lock,.log,.rar,.old,.sql,.sql.gz,.sql.zip,.sql.tar.gz,.sql~,.swp,.swp~,.tar,.tar.bz2,.tar.gz,.txt,.wadl,.zip,.log,.xml,.js,.json,.jpg,.jpeg,.png,.gif,.bmp -w /opt/wordlists/seclists/Discovery/Web-Content/raft-small-directories.txt -recursion
```
- get wordlist from cook
```bash
cook raft-small-directories.txt / | ffuf -c -u $target/FUZZ -w - -recursion
```
- find valid users
```bash
ffuf -c -u $target -w /usr/share/wordlists/SecLists/Usernames/Names/names.txt -X POST -d "username=FUZZ&email=x&password=x&cpassword=x" -H "Content-Type: application/x-www-form-urlencoded" -mr "username already exists"
```
- virtual host discovery (without dns records)
```bash
ffuf -c -u $target -H "Host: FUZZ.$target" -w /opt/wordlists/seclists/Discovery/DNS/subdomains-top1million-5000.txt
```
- get parameter fuzzing
```bash
# param name
ffuf -c -u $target/script.php?FUZZ=test_value -w /path/to/paramnames.txt -fs 4242
# value
ffuf -c -u $target/script.php?param=FUZZ -w /path/to/paramnames.txt -fs 4242
```
- post data fuzzing
```bash
ffuf -X POST -c -u $target/login.php -d "username=admin\&password=FUZZ" -w /path/to/postdata.txt -fc 401
```
- filters

| option | filter      |
| ------ | ----------- |
| -fc    | status code |
| -fl    | line count  |
| -fw    | word count  |
| -fw    | size        |
| -fr    | regex       |

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
- virtual host discovery (without dns records)
```bash
wfuzz -c -z file,/usr/share/seclists/Discovery/DNS/subdomains-top1million-5000.txt -u http://domain.tld -H "Host: FUZZ.domain.tld"
```
- filters

| option | filter               | 
| ------ | -------------------- |
| --hw   | word count           |
| --hc   | response status code |
| --hl   | line count           |