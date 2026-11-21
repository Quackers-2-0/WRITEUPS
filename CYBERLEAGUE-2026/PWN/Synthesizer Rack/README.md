# Challenge Name

Synthesizer Rack

# Description

A synthesizer rack let us create oscillators, filters and other modules, edit them and change their types. It also had a global command-processing function that the program trusted.

# Solution

The main bug was in **Repatch module**. Repatching only changed the saved module type; it didn't rebuild the data already in the heap allocation. That meant I could create an **oscillator**, repatch it as a **filter**, and edit fields that the program would later treat as oscillator data.

A filter label covered the bytes at offsets `0x20` to `0x3f`. But when the same allocation was treated as an oscillator, those bytes included its **output buffer size** and **output buffer pointer**:

```text
filter label bytes  →  oscillator buffer size + pointer
Play module         →  write to that pointer
Record module       →  read from that pointer
```

That gave us a way to read and write memory at chosen addresses.

The **Module info** menu also leaked a function pointer, so the solver could calculate the binary's PIE base. Then the arbitrary read used `puts@GOT` to leak a libc pointer and work out where `system()` was.

The plan was to overwrite a writable global function pointer called `cmd_preprocessor`, **not** the GOT. The program called `cmd_preprocessor()` on menu input before trying to parse it as a number. If that pointer became `system`, typing `cat /flag` would run as a shell command.

My first attempt failed even though the solver said it had overwritten the pointer. The program just replied **Invalid choice** when I typed commands. Looking at the terminal output, the libc address wasn't page-aligned, which was a clue that the offset was wrong.

We corrected the version-specific `puts` offset:

```text
wrong puts offset: 0x87bd0
right puts offset: 0x87cc0
system offset:     0x58750
```

The difference was only `0xf0`, but that was enough to send the overwrite to the wrong address. After using the corrected offset, I got the flag.

This one made type confusion much easier to understand: the program didn't corrupt the allocation when it changed a module's type. It just started **interpreting the same memory in a different way**.

# Flag

```text
CYBERLEAGUE{4ll_h4rd3n3d_st1ll_c0nfus3d}
```

# Solved By

Kazikiri
