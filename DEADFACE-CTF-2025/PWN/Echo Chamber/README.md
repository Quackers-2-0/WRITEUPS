# Challenge Name

Echo Chamber

# Author

@syyntax

# Description

An echoing chat service on `echochamber.deadface.io:13337` mishandles user-controlled strings. Exploit the flaw to reveal a secret flag.

# Solution

The service was supposed to echo whatever I sent it, so I tried feeding it **printf-style format strings** to see if it would treat them as text or as instructions.

My recorded test was:

```bash
echo "%p %p %p" | nc echochamber.deadface.io 13337
```

Instead of printing everything literally, the service returned memory-looking values such as **`0x402000`**. That was a strong sign of a **format-string vulnerability**.

My notes then mention trying `%1$s`, which asks `printf` to treat an argument as a string pointer. The service's output revealed the flag. This was my first clue that even an innocent-looking echo function can leak memory if user input is passed straight to a formatting function.

# Flag

```text
deadface{r3tr0_f0rm4t_l34k_3xp0s3d}
```

# Solved By

Kazikiri
