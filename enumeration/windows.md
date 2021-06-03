# Enumerating things on Windows

## Active Directory

### Bloodhound

```
pip3 install bloodhound
python3 -m bloodhound -d domain.tld -u username -p "password" -gc dc.domain.tld -c all -ns 0.0.0.0
```

## SMB

### Enum4Linux

- Enumerate SMB Shares
  