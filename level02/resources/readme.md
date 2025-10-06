# CTF Report — TCP stream extraction with Wireshark (English, detailed)

---

## Overview

This report documents the exact, unchanged steps you provided for extracting a password from captured TCP traffic in Wireshark. I translate and clarify the wording into English while **preserving your original method, the raw observed bytes, and the final result unchanged**.

---

## Environment / Context

* Tool used: **Wireshark** (analysis of a pcap / TCP streams).
* Goal: locate the place in the capture where a **password** appears and reconstruct the password by taking one character from each subsequent TCP packet payload, then clean non-ASCII markers to reveal the final password.

---

## Steps (translated, step-by-step — solution unchanged)

```
1) Open the capture in Wireshark and locate the packet/field where "password" appears.
2) From that point, for each following TCP packet/connection, take one character (the next character) from the payload.
3) The stream contains dots (.) and other non-ASCII markers; the collected sequence looks like: 
   ft_wandr...NDRel.L0L.
4) Remove the dots up to the last dot (delete the dot characters up to the final dot), but keep the character after the last dot because that corresponds to Enter/newline.
5) After that cleaning step the password extracted is:
   ft_waNDReL0L
```

---

## Raw observed snippet (as you reported)

```
ft_wandr...NDRel.L0L.
```

> This is the literal sequence you observed in the TCP payloads before the cleanup step.

---

## Processing rule (exact logic you applied)

* Remove the dot characters (`.`) **up to the final dot**; keep the character that follows the final dot because it represents the Enter/newline in the captured stream.
* Do **not** change any other characters or reorder anything.

---

## Final extracted password (unchanged)

```
ft_waNDReL0L
```