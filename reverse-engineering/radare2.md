## radare2
- open binary
```bash
r2 -d binary
```
- analyse binary
```bash
aaa
```
- list functions
```bash
afl
```
- dissamble a function
```bash
pdf @function.name
```
- move to a memory location then show the function
```bash
s function.name
pdf
```
- reopen the binary in debug mode
```bash
# without argument
ood
# with argument
ood arg1 arg2
```
- set breakpoint at memory location
```bash
db 0x000000
```
- run the binary
```bash
dc
```