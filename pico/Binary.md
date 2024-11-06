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
