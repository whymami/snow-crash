# CTF Walkthrough — `flag00`

**Challenge Objective:** Gain access to `flag00` user and retrieve the flag.

---

## 1. Full Step Sequence (Verbatim)

```bash
find / -user flag00 2>/dev/null ---> /usr/sbin/john /rofs/usr/sbin/john -----> cat /usr/sbin/john ----> cdiiddwpgswtgt ----> https://www.dcode.fr/caesar-cipher -----> ceaser(11) ----> nottoohardhere ----> su flag00 ----> passwod nottoohardhere ----> getflag ---> Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
```

---

## 2. Step-by-Step Explanation

### 2.1 Search for files owned by `flag00`

```bash
find / -user flag00 2>/dev/null
```

* **Purpose:** Search the entire filesystem for files owned by `flag00`.
* **Explanation:** Suppresses permission errors using `2>/dev/null`.
* **Result:**

```
/usr/sbin/john
/rofs/usr/sbin/john
```

---

### 2.2 Inspect the discovered file

```bash
cat /usr/sbin/john
```

* **Purpose:** View the contents of the file to find a potential password or clue.
* **Result:**

```
cdiiddwpgswtgt
```

---

### 2.3 Decrypt the encoded string

* **Tool used:** [dCode Caesar Cipher](https://www.dcode.fr/caesar-cipher)
* **Method applied:** Caesar cipher with shift of 11 (`ceaser(11)`)
* **Result:**

```
nottoohardhere
```

* **Explanation:** The plaintext string `nottoohardhere` is the password for `flag00`.

---

### 2.4 Switch user to `flag00`

```bash
su flag00
```

* **Password entered:**

```
nottoohardhere
```

* **Explanation:** Successfully switched to the `flag00` user using the recovered password.

---

### 2.5 Retrieve the flag

```bash
getflag
```

* **Output:**

```
Check flag.Here is your token : x24ti5gi3x0ol2eh4esiuxias
```

* **Explanation:** The command `getflag` returns the token associated with `flag00`.

---

## 3. Final Result

**Token / Flag:**

```
x24ti5gi3x0ol2eh4esiuxias
```

---

This is a **full Markdown report** ready to be submitted as a CTF write-up.

I can also create a **single-page PDF-ready version with table of contents, optional screenshots placeholders, and code highlighting** if you want.

Do you want me to do that next?
