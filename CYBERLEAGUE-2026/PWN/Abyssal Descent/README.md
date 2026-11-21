# Challenge Name

Abyssal Descent

# Description

A submarine control program had sonar arrays, a real-time feed and pressure diagnostics. The goal was to use the exposed controls to recover the flag.

# Solution

This one was more complicated than a normal buffer overflow. The supplied binary had **PIE, NX, canaries and Full RELRO**, and its **seccomp** filter blocked the usual shell-spawning syscalls. So even if I controlled execution, `system('/bin/sh')` wasn't going to be the easy way out.

The first useful bug was in the sonar feature. The real-time feed saved a pointer to the sonar array. Later, the program grew that array with `realloc()`, but it didn't update the saved pointer when the allocation moved. That left the feed pointing to **freed memory**: a use-after-free.

The recovered solver, `solve_abyssal.py`, used the same mistake twice:

- First, it grew a large sonar array and read allocator data through the old pointer. That leaked a libc address.
- Then it arranged a smaller allocation so the freed sonar storage got reused as a **pressure sensor object**. The old feed now pointed into that object, letting us read its vtable pointer and work out the PIE base.

Once those addresses were known, the solver used the stale pointer to **replace the pressure object's vtable pointer**. Instead of sending diagnostics to the normal handler, it pointed the vtable at a buffer we controlled and selected the hidden `hw_cal_v1` handler.

That handler had a stack overflow, which gave us a way to build a ROP chain. Because seccomp blocked shell execution, the chain used **open, read and write** instead:

```text
open('/flag', ...)
read(flag_fd, buffer, ...)
write(1, buffer, ...)
```

This is often called an **ORW chain**. It reads the file directly rather than trying to launch a shell.

The saved solver contains the heap setup, libc/PIE leaks, fake vtable and final ORW chain. The chat also records a successful flag, although the complete final terminal output isn't preserved.

This was a good example of several smaller bugs working together. The old pointer gave us leaks and memory control, the vtable got us into the vulnerable function, and the ORW chain worked around seccomp.

# Flag

```text
CYBERLEAGUE{abyssal_0rw_n0_sh3llz_4ll0w3d}
```

# Solved By

Kazikiri
