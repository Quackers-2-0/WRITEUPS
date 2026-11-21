# Challenge Name

Lay of the Land

# Author

@syyntax

# Description

Continue characterizing the Linux host and locate the flag associated with the Hostbusters environment.

# Solution

The challenge was asking me to look around the Linux environment, which made me think of **environment variables**.

I checked the environment belonging to process **PID 1** with this command from my old notes:

```bash
tr '\0' '\n' </proc/1/environ | sed -n '1,200p'
```

The output included the flag. I liked this one because it was a reminder that useful data can be sitting in process information, not just in files under a home directory.

# Flag

```text
deadface{hostbusters4_c6e54afa62741d34}
```

# Solved By

Kazikiri
