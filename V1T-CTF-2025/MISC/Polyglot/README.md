# Polyglot

**Event:** V1t CTF 2025  
**Category:** Misc

## What I did

The challenge gave me a file called `polyglot.png`, but it wasn't only a PNG. Looking inside it showed **more than one file format** packed into the same data.

After extracting the embedded files, I found a **JPG image** and an **MP4 video**. The JPG contained hidden data, but `steghide` needed a password to extract it.

The password was in the MP4. Once I found it, I used `steghide` on the JPG and extracted a text file containing the flag.

The important part was not stopping at the visible PNG. A file can be valid in one format while also carrying other files or data inside it.

## Flag

```text
v1t{duck_l0v3_w4tch1ng_p2r3}
```

## Solved By

Kazikiri
