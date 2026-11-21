# Challenge Name

A Test of Precision

# Description

The program wanted one input to pass two checks. The string `HNF26{Fl4gg3d_F14g}` passed the first check, but wasn't the actual flag.

# Solution

At first I had a string that passed one check, but the program still stopped. Looking at the binary showed that it was checking two different things: the input had to match `HNF26{Fl4gg3d_F14g}`, and another variable had to contain `enabled` instead of `disabled`.

The input was read into a buffer without proper bounds checking, so it was possible to overwrite the next variable. The nice trick was to include a **null byte** after the expected string. That made `strcmp()` stop comparing, even though more bytes followed in memory.

The payload was:

```python
flag = b'HNF26{Fl4gg3d_F14g}'
payload = flag + b'\x00' + b'A' * 19 + b'enabled\x00'
```

The first part passes the string check. The 19 `A` bytes pad the input up to the next variable. Then `enabled\x00` replaces its old value.

With both checks satisfied, the program opened a shell and I got the real flag.

What made this different from ret2win was that I wasn't overwriting a return address. I was changing **data the program used to decide what to do**.

# Flag

```text
HNF26{pr3c1510n_15_c0nt40l}
```

# Solved By

Kazikiri
