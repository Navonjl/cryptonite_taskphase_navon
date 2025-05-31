# IntroToBurp

Flag: `picoCTF{#0TP_Bypvss_SuCc3$S_b3fa4f1a}`

Register on the site, then use burpsuite to intercept the request and remove the data part (i.e. the otp field). Alternatively, use curl to send a request without the otp field:
```sh
curl 'http://titan.picoctf.net:56200/dashboard' \
  -H 'Accept: text/html,application/xhtml+xml,application/xml;q=0.9,image/avif,image/webp,image/apng,*/*;q=0.8' \
  ...
  --data-raw '' \
  --insecure
```

And you get the flag in the response.
Kinda guessy q, but I guess it works.

# Power Cookie

Flag: `picoCTF{gr4d3_A_c00k13_0d351e23}`

Login as a guest. The title talks about cookies, so check the cookies in the browser using the application tab in devtools. There is a cookie called `isAdmin` which is set to 0. Set it to 1 and refresh the page and you get the flag.