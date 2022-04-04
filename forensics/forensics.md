## File operations
#### Extract part of a file
In this example we extract a PNG file from a ZIP file. 
89504e47 = magic number = start of PNG
AE426082 = IEND

```bash
xxd -c1 -p test.png |
  awk -v b="89504e47" -v e="AE426082" '
    found == 1 {
      print $0
      str = str $0
      if (str == e) {found = 0; exit}
      if (length(str) == length(e)) str = substr(str, 3)}
    found == 0 {
      str = str $0
      if (str == b) {found = 1; print str; str = ""}
      if (length(str) == length(b)) str = substr(str, 3)}
    END{ exit found }' |
  xxd -r -p > new_file
test ${PIPESTATUS[1]} -eq 0 || rm new_file
```