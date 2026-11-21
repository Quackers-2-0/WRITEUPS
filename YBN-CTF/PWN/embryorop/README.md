# Challenge Name

embryorop

# Author

Not recorded in the recovered conversation.

# Description

An introductory binary exploitation challenge supplying a Linux x86-64 program and source code (`chal` and `chal.c`). The goal was to gain a shell on the remote instance and read the flag.

# Solution

The source made the buffer overflow pretty clear:

```c
char local[0x100];
void (*func)() = e;
read(0, local, 0x110);
__asm__("jmp *%0" :: "r"(func));
```

The program reads **0x110 bytes** into a **0x100-byte buffer**, and there's a function pointer called `func` on the stack. Instead of overwriting the saved return address like a normal ret2win, I could overwrite `func` because the program jumps to whatever address it contains.

I checked the binary with `checksec`, `nm`, and `ROPgadget`. NX and a stack canary were enabled, PIE was off, and I didn't find a useful `system()` function. A ROP chain using Linux syscalls looked like the better option.

My first payload failed. The mistake was the offset. I assumed `func` was right after the `0x100` buffer, but the disassembly showed an extra eight bytes between them:

```text
buffer:   rbp - 0x110
func:     rbp - 0x008
           -----------
offset:         0x108
```

So the overwrite needed to land at **`0x108`**, not `0x100`.

I fixed the payload with pwntools. The ROP chain went at the start, followed by padding up to `0x108`, then the address of a `ret` gadget. When the program jumped through `func`, that `ret` began running the chain from the stack. It also avoided the normal stack-canary check because the function didn't return through its usual path.

The chain made two syscalls:

```text
read(0, writable_bss, 8)    # write /bin/sh\x00 into memory
execve(writable_bss, 0, 0) # run /bin/sh
```

On x86-64 Linux, `read` uses syscall number `0` and `execve` uses `59`. The arguments go in `rdi`, `rsi`, and `rdx`.

The key part of the working payload was:

```python
TOTAL_READ = 0x110
OFF_FUNC = 0x108

payload = chain.ljust(OFF_FUNC, b'A')
payload += p64(ret_gadget)
assert len(payload) == TOTAL_READ
```

I sent the fixed payload to the remote service, followed by `/bin/sh\x00`, and got the flag. This was a good reminder to check the actual disassembly instead of guessing the stack layout from the C source.

# Flag

```text
YBN25{this_should_have_been_a_nice_warmup}
```

# Solved By

Kazikiri
