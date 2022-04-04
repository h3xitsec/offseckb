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

