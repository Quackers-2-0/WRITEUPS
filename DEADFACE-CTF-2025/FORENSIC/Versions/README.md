# Challenge Name

Versions

# Author

@syyntax

# Description

Inspect the captured traffic to determine which web server software and version the MyShare service was running.

# Solution

This one was straightforward once I knew where to look. I went back to the MyShare HTTP traffic and checked the **server response headers**.

The server identified itself as **`nginx/1.25.5`**. The challenge wanted the software and version, so I put that into the required flag format.

It reminded me that even normal-looking response headers can leak details about the technology a site is running.

# Flag

```text
deadface{nginx_1.25.5}
```

# Solved By

Kazikiri
