Summary

The following is a step-by-step record of the actions performed and console outputs — unchanged and ready to paste.

Steps & Commands
1. Read /etc/passwd and copy hash
cat /etc/passwd ----> copy passwd -----> nano hash ----> paste passwd ---> john hash

2. Run John the Ripper
Loaded 1 password hash (descrypt, traditional crypt(3) [DES 128/128 SSE2])
No password hashes left to crack (see FAQ)
whymami@whymami:~$ john --show hash
flag01:abcdefg:3001:3001::/home/flag/flag01:/bin/bash

3. Switch user to flag01
su flag01 ----> abcdefg

4. Get flag
getflag ----> Check flag.Here is your token : f2av5il02puano7naaf6adaaf