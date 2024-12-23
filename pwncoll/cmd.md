# Chaining Commands

All of this can be cheesed with just simple PATH attcks on ``fold``, but :

## Chaining commands with Semicolons

``/challenge/pwn;/challenge/college``

## Your first shell script
Use vim to create the shell script asked, and then run it. 
## Redirecting Script output
run it in a subshell (``$(foo;bar)``), and pipe it

## Executable Shell Scripts

we first have to create a shell script that will invoke /challenge/solve on running. we do this by doing **echo /challenge/solve > x.sh**. we then have to make it executable to our user without using bash. as we learnt in the previous module, we do this by doing **chmod +x x.sh**, and then run it with ./x.sh to get the flag.
