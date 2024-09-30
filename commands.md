
Evrything was rather unremarkable until "more catting practice":
```bash
Connected!                                                                        
You cannot use the 'cd' command in this level, and must retrieve the flag by absolute path. Plus, I hid the flag in a different directory! You can find it in the file /usr/share/maven-repo/flag. Go cat it out **without** cding into that directory!
# Oh, sure. challenge accepted.
hacker@commands~more-catting-practice:~$ cd
You used 'cd'! In this level, I don't allow you to change the working directory --- you MUST chase pass 'cat' the absolute path of where I put it on the filesystem (which is /usr/share/maven-repo/flag).

hacker@commands~more-catting-practice:~$ type -a cd
cd is a function
cd () 
{ 
    [ "${#FUNCNAME[@]}" -gt 1 ] && return;
    fold -s <<< "You used 'cd'! In this level, I don't allow you to change the working directory --- you MUST chase pass 'cat' the absolute path of where I put it on the filesystem (which is $(cat /tmp/.flag-path 2> /dev/null)/flag)."
}
cd is a shell builtin

# rembers that cd is a shell builtin smh
hacker@commands~more-catting-practice:~$ builtin cd /usr/share/maven-repo/
hacker@commands~more-catting-practice:/usr/share/maven-repo$ cat flag
# <insert flag here>
```
which was fun lol
