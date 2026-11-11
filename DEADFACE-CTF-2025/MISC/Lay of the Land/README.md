# Challenge Name

Lay of the Land

# Author

@syyntax

# Description

Continue characterizing the Linux host and locate the flag associated with the Hostbusters environment.

# Solution

1. The wording about characterizing the environment pointed me toward environment variables.
2. I inspected the environment of PID 1 using the command recorded in my original notes:

   ```bash
   tr '\0' '\n' </proc/1/environ | sed -n '1,200p'
   ```

3. The output contained the flag.

# Flag

```text
deadface{hostbusters4_c6e54afa62741d34}
```

# Solved By

Kazikiri
