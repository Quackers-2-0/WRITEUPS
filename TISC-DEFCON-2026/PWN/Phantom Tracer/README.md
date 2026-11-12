# Challenge Name

Phantom Tracer

# Description

The service was a heap challenge with a simple menu to allocate, edit, free and scan chunks. I had the binary, its matching `libc.so`, and access to the remote service. The goal was to turn a memory bug into a shell and read the flag.

# Solution

The menu looked straightforward, but the index handling wasn't. The program stored chunk pointers and sizes in global arrays, and the `edit` function accepted a **negative index** without a proper check.

The saved binary layout was:

```text
__dso_handle = 0x4008
chunks       = 0x4060
sizes        = 0x40c0
```

That put `__dso_handle` just `0x58` bytes before `chunks`. Using `edit(-11, ...)` let me write outside the intended chunk array and change the pointers the menu used. Once a chunk entry pointed somewhere else, the normal **scan** and **edit** options became ways to read and write other memory.

I still needed to find libc's base address because ASLR moves it around. The easiest leak was from a large freed chunk. I allocated a chunk of size `0x438`, freed it, and scanned it. Its unsorted-bin metadata contained a libc pointer. With the provided libc, the relation was:

```python
libc_base = unsorted_fd - 0x203b20
```

That gave me the addresses needed for the next part.

I tried several ways to turn the memory access into code execution. Reading the stack to work out the pointer guard kept killing the remote connection. Overwriting `__free_hook` with `system` also didn't work, even though the symbol was in the supplied libc. The older hook trick wasn't useful here.

I moved to **FILE structure exploitation**, using a House-of-Apple-style attack against the live `stdout` object. The important libc offsets recorded in the workspace were:

```text
_IO_2_1_stdout_ = libc + 0x2045c0
_IO_wfile_jumps = libc + 0x202228
system          = libc + 0x58750
stdout_lock     = stdout + 0x1150
```

The idea was to corrupt the `stdout` structure so that glibc's normal cleanup would follow a controlled wide-file callback. I used the real `_IO_wfile_jumps` vtable and arranged the fake wide-data structures so that a callback would reach `system`.

This was fussy. My first layouts used the wrong field offsets, and pointing `chunk0` at `stdout` made the program die too early. The more stable approach was to leave `chunk0` alone, repoint **`chunk6`** at `stdout`, then write the forged FILE structure through `edit(6, payload)`.

The command also had to stay short, or it would clash with the fields of the forged structure. The working string was:

```text
aa;sh 1>&2;#
```

It tries `aa` first, starts a shell, and sends the shell's output through file descriptor 2 because `stdout` has been corrupted. Sending `x` to the menu then triggered the exit path and glibc cleanup.

The response included:

```text
sh: 1: aa: not found
```

That error was actually good news: it meant the command had reached a shell. After that I could send commands such as `id`, `pwd`, `ls` and `cat flag.txt` over the same connection. The saved writeup records the returned flag below.

The workspace contains quite a few failed solver versions. `solve7.ps1` is the saved variant that matches this route, but the existence of the other scripts doesn't mean they all worked.

# Flag

```text
TISCDCSG{vofdVmzO4ypb4YE0UemhJM0zQoY3DrL1}
```

# Solved By

Kazikiri
