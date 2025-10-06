# CTF Report: SnowCrash Level13

## Summary
UID check bypass using GDB. The `level13` binary validates UID against hardcoded value `0x1092` (4242). By manipulating the EAX register during runtime with GDB, the check is bypassed to reveal the token.

**Flag:** `2A31L79asukciNyi8uppkEuSx`

## Analysis

### Initial Execution
```bash
./level13
UID 2013 started us but we we expect 4242
```

Binary expects UID 4242 but current UID is 2013.

### Disassembly Analysis
```bash
gdb ./level13
(gdb) disassemble main
```

**Critical Instructions:**
```asm
0x08048595 <+9>:     call   0x8048380 <getuid@plt>
0x0804859a <+14>:    cmp    $0x1092,%eax
0x0804859f <+19>:    je     0x80485cb <main+63>
```

**Control Flow:**
1. `getuid()` called - returns current UID in EAX
2. EAX compared with `0x1092` (4242 in decimal)
3. If equal, jumps to success path at `0x80485cb`
4. Otherwise, prints error and exits

**Success Path:**
```asm
0x080485cb <+63>:    movl   $0x80486ef,(%esp)
0x080485d2 <+70>:    call   0x8048474 <ft_des>
0x080485d7 <+75>:    mov    $0x8048709,%edx
0x080485dc <+80>:    mov    %eax,0x4(%esp)
0x080485e0 <+84>:    mov    %edx,(%esp)
0x080485e3 <+87>:    call   0x8048360 <printf@plt>
```

If UID check passes, `ft_des()` is called and token is printed.

## Vulnerability

**Type:** Logic Flaw - Client-Side UID Validation

**Root Cause:** 
- UID check performed in user-controllable process
- No kernel-level privilege enforcement
- Debugger can modify runtime values

## Exploitation

### GDB Register Manipulation

```bash
gdb ./level13
```

**Step 1: Set Breakpoint After getuid()**
```gdb
(gdb) break *0x0804859a
Breakpoint 1 at 0x804859a
```

Breakpoint set right after `getuid()` call, before comparison.

**Step 2: Run Program**
```gdb
(gdb) run
Starting program: /home/user/level13/level13

Breakpoint 1, 0x0804859a in main ()
```

**Step 3: Modify EAX Register**
```gdb
(gdb) set $eax=0x1092
```

Overwrites EAX value with expected UID (4242).

**Step 4: Continue Execution**
```gdb
(gdb) continue
Continuing.
your token is 2A31L79asukciNyi8uppkEuSx
[Inferior 1 (process 19264) exited with code 050]
```

Token successfully retrieved!

## Attack Chain

1. Binary calls `getuid()` returning 2013
2. GDB breakpoint hits before comparison
3. EAX register manually set to 0x1092 (4242)
4. Comparison succeeds: `cmp $0x1092, %eax` → equal
5. Jump to success path taken
6. `ft_des()` function decrypts/generates token
7. Token printed to stdout

## Mitigation

1. **Kernel-Level Checks:** Use `setuid` bit and kernel enforces privileges
2. **Anti-Debugging:** Implement ptrace detection
3. **Code Obfuscation:** Make reverse engineering harder
4. **Server-Side Validation:** Move privilege checks to trusted environment
5. **Integrity Checks:** Detect debugger attachment and memory modification
6. **Hardware Security:** Use secure enclaves (TPM, SGX)


## Timeline
- Discovered UID validation error
- Disassembled binary to locate check
- Identified comparison at `0x0804859a`
- Set breakpoint before comparison
- Manipulated EAX register to expected value
- Successfully bypassed check and captured token