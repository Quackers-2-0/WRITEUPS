# Challenge Name

Greenhouse Monitor

# Description

A greenhouse-monitoring program let us read sensor logs, check the firmware version and change sensor settings. The goal was to reach the flag stored inside the program.

# Solution

The binary had the usual protections: **PIE, NX, a stack canary and Full RELRO**. At first that looked like a lot to work around, but two menu options were leaking information we needed.

The **View sensor log** option cleared only 64 bytes of a stack buffer but wrote **80 bytes** back to us. The last eight bytes of that output included the **stack canary**. In the exploit we worked out, the canary was at offset 72:

```python
canary_bytes = leak[72:80]
```

The **Firmware version** option also printed a runtime pointer to `main`. Subtracting `main`'s offset in the binary gave the PIE base:

```python
pie_base = leaked_main - 0x1793
```

That was enough to work out the addresses of useful code and the global `flag` buffer. The important offsets from the supplied binary were:

```text
pop rdi; ret : 0x1229
puts@plt     : 0x1050
flag         : 0x4060
```

The actual overflow was in **Set sensor name**. It read up to 128 bytes into a stack buffer whose canary was just 72 bytes from the start. Normally that would cause a stack-smashing error, but we already knew the canary's value.

The payload kept the canary intact and replaced the return address with a tiny ROP chain:

```text
72 bytes of padding
leaked 8-byte canary
8 bytes for saved RBP
pop rdi; ret
address of flag
puts@plt
```

That made the program call `puts(flag)` and print the flag directly. We didn't need to leak libc or spawn a shell.

The full solver is preserved in the old conversation, but I don't have a separate copy of the final script in this folder. I did report the successful flag after using the approach.

I liked this one because the protections weren't switched off. The program simply leaked enough information for us to get around them.

# Flag

```text
CYBERLEAGUE{c4n4ry_p13_r0p_tr1pl3_thr34t}
```

# Solved By

Kazikiri
