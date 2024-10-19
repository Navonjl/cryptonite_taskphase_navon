# Perceiving Permissions
Probably the most annoying one as I mad my perms reset every other time even from the smallest typos. So annoying.

## Changing File Ownership

Go into root with **cd /**, then use chown hacker flag to allow the hacker user to access flag. then we can simply **cat flag** to get the flag.

## Groups and Files

run **chgrp hacker flag** to change the permissions of the flag command to allow the hacker group to access it.

## Fun with group names

Just use **id** and **chgrp hacker (new group name)**.

## Changing Permissions

we learn about chmod, which i've used a lot previously to make bash scripts executable. it tells us about the r, w, x arguments to it, which make the file readable, writable and executable. i go to / and do **chmod +rwx ./flag** (although just a +r would suffice), which gives the current user the permission to read the file. i can then get the flag from it.

## Executable files

We just have to make the /challenge/run file executable. any linux user would've done this a lot of times, we just do a simple **chmod +x /challenge/run**.

## Permission Tweaking Practice

nearly lost my sanity here ngl.
TODO: any way to cheese this ? I could cheese most other levels, but this one was hard. Looking at the script used here, twas python writing state to a file.
I couldn't modify the state to smthing I liked, since it was owned by root, but it seemed to just delete the file at a mistake and next time work with a custom inital state.
You _could_ somehow modify the state post a mistake to fool the script, but I realised that it was too much work (ran into perm errors or smth) and got too lazy : P. 

```
chmod g-r ./challenge/pwn 
chmod g+rx,a+r,u+x ./challenge/pwn
chmod u-rwx ./challenge/pwn
chmod u+rw,g+w,a+w ./challenge/pwn
chmod u-rw,g-rw ./challenge/pwn
chmod g-x ./challenge/pwn
chmod a-w ./challenge/pwn
```

after this, we can finally change flag to be readable by doing **chmod +r /flag** and print it out for the flag.

## Permissions Setting Practice

The same thing, but we use = instead here, which makes it faster since we don't have to see the difference between the current and required permissions.
```
chmod u=r,g=rw,o=x /challenge/pwn
chmod u=,g=rwx,o=rw /challenge/pwn
chmod u=w,g=rwx,o=x /challenge/pwn
chmod u=,g=w,o=wx /challenge/pwn
chmod u=wx,g=,o=r /challenge/pwn
chmod u=rwx,g=wx,o=wx /challenge/pwn
chmod u=r,g=w,o=rwx /challenge/pwn
chmod u=rw,g=wx,o=x /challenge/pwn
```

## The SUID Bit
run ``chmod u+s /challenge/getroot``, sets a suid bit for the current user on getroot. then we execute /challenge/getroot, which spawns a root shell.
