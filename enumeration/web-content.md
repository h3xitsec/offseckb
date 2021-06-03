# Web Content Enumeration

## Dirb

- Find folders
```
dirb http://hostname /usr/share/seclists/Discovery/Web-Content/common.txt -o </path/to/output/file.txt>
```
## Gobuster

- Find specific file type
```
gobuster dir -u http://www.domain.tld -t 100 -e -x php,html,js,txt -w /usr/share/seclists/Discovery/Web-Content/directory-list-2.3-big.txt
```