# Challenge Name

Secret Stash

# Author

@syyntax

# Description

Look for hidden files left by the novice DEADFACE user on the remote Linux host, and recover the flag.

# Solution

1. I listed the hidden directory entries using:

   ```bash
   ls -a
   ```

2. I found a hidden entry named `.dont_forget`.
3. Inspecting that hidden file revealed the flag.

# Flag

```text
deadface{hostbusters2_4685d0c801939781}
```

# Solved By

Kazikiri
