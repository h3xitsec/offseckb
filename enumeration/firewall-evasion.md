## nmap
```bash
# Decoy
-D x.x.x.x,y.y.y.y,ME
# Decoy with random ip
-D RND,RND,ME
# Use socks4 proxy
--proxies proxyurl
# Source MAC Spoofing
--spoof-mac xx:xx:xx:xx:xx
# Source IP Spoofing
-S x.x.x.x
# Specific source port
-g 80
--source-port 80
# Fragment with 8 bytes of data
-f
# Fragment with 16 bytes of data
-ff
# Fragment with MTU (multiple of 8)
--mtu 8 # identical to -f
# Generate packets with specific length (multiple of 8)
--data-length 8
# Set TTL
--ttlp
# Use bad checksum
--badsum
```