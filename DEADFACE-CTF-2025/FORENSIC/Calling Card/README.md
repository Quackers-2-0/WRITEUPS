# Challenge Name

Calling Card

# Author

@syyntax

# Description

A PCAP captures the initial probing of the MyShare web application. Find the hidden calling-card flag in the HTTP traffic.

# Solution

1. I opened the supplied packet capture and inspected the HTTP requests.
2. I followed the relevant HTTP exchange and looked beyond the request path and body.
3. A nonstandard/custom HTTP header contained the flag.

# Flag

```text
deadface{l3ts_get_Th3s3_fiL3$}
```

# Solved By

Kazikiri
