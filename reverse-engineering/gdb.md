## gdb
- open binary
```bash
gdb binary
```
- open binary - tui mode
```bash
gdb-pwndbg -tui binary
set disassembly-flavor intel
layout asm
layout reg
```
- list functions
```bash
info functions
info functions regexp
```
- dissamble a function
```bash
disass
```
- generate cyclic pattern
```bash
# pwndbg
cyclic 100
```
- set breakpoint at memory location
```bash
b *0x000000
```
- step line by line
```bash
stepi
```
- show cpu registers
```bash
# show all registers
bt
```
- run python code within gdb
```bash
## exemples
# use python output as input for binary
> shell python -c "print('A' * 28 + 'B' * 4)" > input
> run < input

# within gdb commands
> run $(python -c "print('A' * 64)")

# oneliner
python print('A' * 64)

# multiline
python
> pay = 'pay'
> load = 'load'
> payload = pay+load
> print(payload)
> end
payload

# gdb within python within gdb
python import gdb
python gdb.execute('run')
```
- find EIP offset
```
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
