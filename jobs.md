# Processes and Jobs

## Listing Processes

The ps command lists all active processes. It doesn't actually list all process though, Only the processes running in the current terminal. ps -ef displays ALL the processes running on the machine, with the e part showing all the processes, and the f giving additional information.

**ps -ef**, and the challenge file is indeed running in the background. it's running as **/challenge/28369-run-24163**. i run this file in the shell and it yields me the flag.

## Killing Processes

No ``killall`` here, so we use ``ps -ef`` and kill to kill the process.

## Interrupting Processes

Use ctrl-c, or send the equivalent code via kill -SIGINT.

## Suspending Processes

we run /challenge/run and then suspend the process (pause it) by doing ctrl-z (SIGTSTP) to get the flag.
You only need kill for everything. Pheraps violence _is_ the solution.
(/s ofc)

## Resuming Processes

We run and suspend the same process again, with ctrl-z. then we resume the previous process by doing fg to yield us the key.

## Background Processes

We run the program and suspend it. we can then resume and allow it to run in the background by using the bg command. since it's running in the background, we can still use our terminal and run /challenge/run again to get the flag.

## Foreground Processes

We run /challenge/run, and it starts up the file. we have to pause and suspend it first by doing ctrl-z. we are then supposed to put it in the background, THEN bring it into the foreground to get the flag. we do this by first doing **bg** to resume the process in the background.

our terminal's still usable, so now we bring it into the foreground with **fg**. it yields us the flag.

## Starting Backgrounded Processes

Just do ``/challenge/run &`` to run the program in the background and get the flag.

## Process Exit Codes

run ``/challenge/get-code``, then ``echo $?`` which yields us the flag. 
