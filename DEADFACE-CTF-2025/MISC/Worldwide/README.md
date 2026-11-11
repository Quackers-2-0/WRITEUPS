# Challenge Name

Worldwide

# Author

@syyntax

# Description

Investigate the other accounts and shell configuration on the Hostbusters machine to find the next flag connected to user `deephax`.

# Solution

I obtained this flag **incidentally**, while looking for a different Hostbusters flag; I did not follow the intended lateral-movement route.

1. While searching the system for flag strings, I ran the recursive grep command recorded in my notes:

   ```bash
   grep -Rns "deadface{" / 2>/dev/null || true
   ```

2. The search found the Worldwide flag.

The challenge’s intended route reportedly involved the `deephax` account, but that is **not** how I solved it; I am preserving my actual approach.

# Flag

```text
deadface{hostbusters5_e16a5c8995620a24}
```

# Solved By

Kazikiri
