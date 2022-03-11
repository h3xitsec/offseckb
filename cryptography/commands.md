## file to hex
`cat file | xxd -p | tr -d '\n'`

## show public key details
```bash
openssl rsa -noout -text -inform PEM -in file.pub -pubin
RsaCtfTool --dumpkey --key file.key
```