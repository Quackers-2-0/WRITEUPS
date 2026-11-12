# Challenge Name

ret2win

# Description

The binary said it checked the input length to prevent an overflow. The goal was to reach its hidden `win()` function anyway.

# Solution

I started in **Ghidra** and looked at `main()` and the `win()` function. The stack buffer began at `RBP - 0x70`, so I worked out an offset of **120 bytes** to the return address: 112 bytes for the local space and another 8 for the saved base pointer.

The `win()` function was at `0x4011d7` and would launch a shell if I could reach it.

I tried sending a cyclic pattern in GDB, but the program exited before crashing:

```text
Can't buffer overflow if i check for the length first!
Hey no funny business!
```

So the length check was stopping my obvious overflow. The trick was to put a **null byte at the start** of the input. Functions such as `strlen()` stop at a null byte, but the rest of the bytes can still be present in the buffer. That let the payload pass the length check while still overwriting the return address.

The important part of the working pwntools payload was:

```python
from pwn import *

exe = ELF('./chall_patched')
win = exe.sym['win']

payload = b'\x00' + b'A' * 119 + p64(win)
```

`exe.sym['win']` looks up the `win()` function's address from the binary, and `p64()` packs that address into eight bytes. The script sent the payload to the challenge process and reached the hidden function.

After I got the flag, I went back through the script to learn what each pwntools function was doing. This one taught me that **passing a length check doesn't mean all the bytes after a null are harmless**.

# Flag

```text
HNF26{l3ngth_ch3ck_wh0?}
```

# Solved By

Kazikiri
