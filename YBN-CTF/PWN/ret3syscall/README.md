# Challenge Name

ret3syscall

# Author

Not recorded in the recovered conversation.

# Description

An AArch64/ARM64 binary exploitation challenge, accompanied by source code and instructions for running the binary under `qemu-aarch64`. The goal was to construct a ROP chain that spawns a shell.

# Solution

This was my first big hurdle: I was dealing with an **ARM64 binary**, but my debugger was looking at the x86-64 `qemu-aarch64` process instead of the program running inside QEMU. That's why my breakpoints and disassembly didn't make sense.

The suggested fix was to use QEMU's GDB port with an ARM64-aware debugger:

```bash
qemu-aarch64 -g 1234 ./chal
# In another terminal:
gdb-multiarch ./chal
# In GDB:
set architecture aarch64
target remote :1234
```

I moved on to the remote exploit rather than spending more time debugging locally.

The overflow was in the source:

```c
char max[10];
read(0, max, 0x300);
```

That's up to **768 bytes going into a 10-byte buffer**. The disassembly showed that the saved return address (`x30`) could be reached at offset **`0x18`**, with the rest of the ROP chain starting around **`0x30`**.

ARM64 ROP works differently from the x86-64 examples I was used to. Arguments are passed through `x0`, `x1`, and `x2`, and the syscall number goes in `x8`. For `execve`, that number is **221**.

The chain used the following gadgets to put `/bin/sh\x00` in writable memory, set up the arguments, and make the syscall:

```text
0x430418   load x0, x1, x16 and w6 from the stack
0x427094   str x0, [x1]         -> write /bin/sh to .bss
0x41112c   set x2 = 0           -> NULL environment
0x442990   mov x8, x6; svc #0  -> make syscall

.bss address: 0x4b2150
execve syscall number: 221
```

I also had to account for how much each gadget moved the stack pointer. It wasn't enough to just put the addresses next to each other.

After trying the payload against the remote service, I got a shell and read the flag. The biggest thing I learned here was how different the registers and stack movements can be when moving from x86-64 to ARM64.

# Flag

```text
YBN25{one_small_step_for_man_one_giant_leap_for_mankind_123942}
```

# Solved By

Kazikiri
