# Challenge Name

Versions

# Author

@syyntax

# Description

Inspect the captured traffic to determine which web server software and version the MyShare service was running.

# Solution

1. I returned to the same HTTP exchange inspected for the earlier MyShare challenges.
2. I read the server-identifying response header.
3. The header reported `nginx/1.25.5`, which I submitted in the expected flag format.

# Flag

```text
deadface{nginx_1.25.5}
```

# Solved By

Kazikiri
