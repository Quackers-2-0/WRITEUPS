# Challenge Name

Echo Chamber

# Author

@syyntax

# Description

An echoing chat service on `echochamber.deadface.io:13337` mishandles user-controlled strings. Exploit the flaw to reveal a secret flag.

# Solution

1. I tested how the service handled printf-style format specifiers using the following probe:

   ```bash
   echo "%p %p %p" | nc echochamber.deadface.io 13337
   ```

2. The response included a pointer such as `0x402000`, indicating that the input was being interpreted as a format string rather than echoed literally.
3. My historical notes record trying `%1$s` to have the service print a string from memory. The service returned the flag in its `Echo:` output.

The original notes mention an alternative pointer-injection approach but do not show it being performed, so I have not presented that as part of my solve.

# Flag

```text
deadface{r3tr0_f0rm4t_l34k_3xp0s3d}
```

# Solved By

Kazikiri
