# Challenge Name

Vinyl Scratch

# Description

The challenge had a record-review portal that read our review back to us. The hidden flag could be printed by a function in the binary, but the normal menu didn't call it.

# Solution

The review feature had a classic **format-string vulnerability**. Instead of printing our text safely with:

```c
printf("%s", review);
```

it effectively did:

```c
printf(review);
```

That meant input such as `%p` wasn't treated as normal text. It made `printf()` read values from memory.

The first part of the exploit was to find a useful pointer on the stack. The old analysis found that `%25$p` leaked a return address inside the program, so subtracting its known offset gave the **PIE base**:

```python
pie_base = leaked_return_address - 0x1325
```

The binary also had a handy function called `print_flag()` and a writable global pointer called `exit_handler`. Normally, the exit menu called `normal_exit()` through that pointer. We wanted it to call `print_flag()` instead.

The offsets in the challenge binary were:

```text
normal_exit : PIE + 0x1450
print_flag  : PIE + 0x1570
exit_handler: PIE + 0x4010
```

Because the two functions were so close together, we didn't need to overwrite the whole pointer. We could use **`%hn`**, which writes two bytes, to change the low part of `exit_handler` to point at `print_flag()`.

After that, selecting **Exit** would call the flag function instead of the normal exit handler. The historical chat contains the proposed pwntools exploit, and I confirmed the flag after using the approach.

The binary had Full RELRO, but that didn't stop this attack because we weren't trying to overwrite the GOT. We were writing to a different global function pointer that was still writable.

# Flag

```text
CYBERLEAGUE{v1nyl_f0rm4t_wr1t3_p13_c0mb0}
```

# Solved By

Kazikiri
