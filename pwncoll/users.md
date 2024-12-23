# Untangling Users

## Becoming root with su

run ``su``

## Other users with su

we pass the argument of the user that we want to switch to, in this case zardus with **su zardus**. after inputting the given password, we run /challenge/run to get the flag.

## Cracking passwords

I used hashcat to crack the hash (HAHAHA GPUS GO BRRRRRR). This route led to some insanity, with me running into linux kernel null derefs. (The nvidia gpu on linux memes are real)
Finally got a dynamic gpu loading setup up tho, and had the hash cracked in a second (takes ~1 min on cpu.)

now we su back into zardus with this password and run /challenge/run to get the flag.

## Using sudo

since we can run commands with root access with sudo, we do a **sudo cat /flag** and get the flag.
