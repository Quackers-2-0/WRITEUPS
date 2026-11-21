# Waddler

**Event:** V1t CTF 2025  
**Category:** Pwn

## What I did

I downloaded the supplied binary and started with `file`, `strings`, and `objdump`. That showed calls such as `fopen`, `fgets`, and `printf`, plus a hidden function that printed the flag.

In `main`, the program reserved **0x40 bytes** for an input buffer but called `fgets` with a size of **0x50 bytes**. That meant I could write past the buffer and overwrite the saved return address.

The hidden `duck()` function was at `0x40128c`. It opened `flag.txt`, read its contents, and printed them. So instead of injecting shellcode, I just had to redirect execution to that function.

The offset was **72 bytes**: 64 bytes for the buffer and another 8 for the saved base pointer. This is the payload I saved in my notes:

```bash
python3 - <<'PY' | nc -q 1 chall.v1t.site 30210
import sys
sys.stdout.buffer.write(b'A'*72 + (0x40128c).to_bytes(8, 'little')[:7])
PY
```

It fills the buffer with `A`s and writes the function address in little-endian format. This was a straightforward **ret2win** challenge, and the useful lesson was how to turn a buffer-size mistake into a jump to an existing function.

## Flag

```text
v1t{w4ddl3r_3x1t5_4e4d6c332b6fe62a63afe56171fd3725}
```

## Solved By

Kazikiri
