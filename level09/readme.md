level09@SnowCrash:~$ ls
level09  token


./level09 a
>  a

level09@SnowCrash:~$ ./level09 abc
ace


level09@SnowCrash:~$ cat test.c
#include <stdio.h>


int main(int arc, char **argv)
{
        int i = -1;
        while (argv[1][++i])
                printf("%c", argv[1][i] - i);
        printf("\n");
}


 gcc test.c
level09@SnowCrash:~$ ./a.out "f5mpq;v�E��{�{��TS�W�����"
f4kmm6p跴;䳰ᰭjݬ�f٨�֥�86Ѡ�6͜�ʙ�ǖ�ē����
level09@SnowCrash:~$ ./a.out $(cat token)
f3iji1ju5yuevaus41q1afiuq

level09@SnowCrash:~$ su flag09
Password:
Don't forget to launch getflag !
flag09@SnowCrash:~$ getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z