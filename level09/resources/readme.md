# CTF Report: SnowCrash Level09

## Summary
Reverse engineering a custom encoding algorithm. The `level09` binary encodes input by adding each character's index to its ASCII value. Created a decoder to retrieve the flag09 password from an encrypted token file.

**Flag:** `s5cAJpM8ev6XHw998pRWG728z`

## Analysis

### Binary Behavior
```bash
./level09 a
>  a

./level09 abc
ace
```

**Pattern Discovery:**
- Input: `a` → Output: `a` (a + 0)
- Input: `abc` → Output: `ace` (a+0, b+1, c+2)
- Each character is shifted by its position index

### Reverse Engineering

**Encoding Algorithm:**
```
output[i] = input[i] + i
```

**Decoding Algorithm:**
```c
#include <stdio.h>

int main(int arc, char **argv)
{
        int i = -1;
        while (argv[1][++i])
                printf("%c", argv[1][i] - i);
        printf("\n");
}
```

## Exploitation

### Step 1: Analyze Token File
```bash
cat token
f5mpq;v�E��{�{��TS�W�����
```

Encrypted password stored in token file.

### Step 2: Create Decoder
```bash
cat test.c
#include <stdio.h>

int main(int arc, char **argv)
{
        int i = -1;
        while (argv[1][++i])
                printf("%c", argv[1][i] - i);
        printf("\n");
}

gcc test.c -o decoder
```

### Step 3: Decode Token
```bash
./a.out $(cat token)
f3iji1ju5yuevaus41q1afiuq
```

**Decoded Password:** `f3iji1ju5yuevaus41q1afiuq`

### Step 4: Login and Get Flag
```bash
su flag09
Password: f3iji1ju5yuevaus41q1afiuq
getflag
Check flag.Here is your token : s5cAJpM8ev6XHw998pRWG728z
```

## Vulnerability

**Type:** Weak Cryptography / Custom Encoding

**Root Cause:** 
- Simple positional encoding (Caesar cipher variant)
- Easily reversible algorithm
- Token file world-readable
- No proper encryption used

**Attack Vector:** Reverse engineer encoding and decode stored credentials.

## Mitigation

1. Use strong encryption (AES, RSA) instead of custom encoding
2. Implement proper key management
3. Restrict file permissions on sensitive files
4. Use hashing with salt for password storage
5. Never rely on security through obscurity
6. Implement proper cryptographic libraries

## Timeline
- Analyzed binary behavior with test inputs
- Identified positional encoding pattern
- Reverse engineered the algorithm
- Created decoder program
- Successfully decoded token file
- Obtained flag09 password
- Logged in as flag09 and captured flag