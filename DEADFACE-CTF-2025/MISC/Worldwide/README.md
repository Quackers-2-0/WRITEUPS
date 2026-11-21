# Challenge Name

Worldwide

# Author

@syyntax

# Description

Investigate the other accounts and shell configuration on the Hostbusters machine to find the next flag connected to user `deephax`.

# Solution

I found this one **by accident** while I was looking for another Hostbusters flag.

Instead of following the intended route involving the `deephax` account, I searched the files on the machine for anything that looked like a DEADFACE flag:

```bash
grep -Rns "deadface{" / 2>/dev/null || true
```

That search found the Worldwide flag too. It wasn't the challenge's intended method, but it was how I actually got the answer. Sometimes a broad search pays off before you've even worked through the proper route.

# Flag

```text
deadface{hostbusters5_e16a5c8995620a24}
```

# Solved By

Kazikiri
