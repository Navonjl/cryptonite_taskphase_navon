# GDB Baby steps

**Flag:** `picoCTF{549698}`

So I solved this in nearly 3 different ways:

- fire up ida64 and throw the binary at it, giving me my first succesfull RE flag (with a bit of help from chatgpt, as I didnt know what the h in ``86342h`` meant.)

Probably the easiest way to do it - cyberchef did gimme some pain trying to do hex to decimal, so used another tool.

- ``disassemble main``

open up gdb and run that - ez.

Now for the dynamic methord - _proper_ gdb

This took me a fair minute to crack.

- dynamic analysis with gdb

So, if you are stupid like me, you probably tried something to the effect of -
```gdb
(gdb) break main
(gdb) start
(gdb) i r eax
eax            0x55555129          1431654697
// hold up whaaaa
```

you try a few more times and slowly start doubting your sanity. Is it really decimal ? is this some Big endian / little  endian bullshit ?

Nope, It's just that breakpoints are only set at the start of a function. Of course.
You lurk around more and find that you can move at the instruction level (huh) using ``ni`` (next instruction). (Use layout asm (_after_ disassemble main)) for more cool points)

After _all_ that, you get your flag.

References:
chatgpt, Stackoverflow and murking around.

# Vault door 3

**Flag:** `picoCTF{jU5t_a_s1mpl3_an4gr4m_4_u_79958f}`

There is liteally no steps here, just reverse it using a bitta python

```py
s = "jU5t_a_sna_3lpm18g947_u_4_m9r54f"

buffer = ["z"] * len(s)

for i in range(8):
    buffer[i] = s[i];

for i in range(8,16):
    buffer[23-i] = s[i];

for i in range(16,32,2):
    buffer[46-i] = s[i];

for i in range(17, 32,2):
    buffer[i] = s[i]

print(''.join(buffer))
```
There u go.
