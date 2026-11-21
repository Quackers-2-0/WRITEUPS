# Challenge Name

Calling Card

# Author

@syyntax

# Description

A PCAP captures the initial probing of the MyShare web application. Find the hidden calling-card flag in the HTTP traffic.

# Solution

I opened the supplied packet capture and started looking through the HTTP requests. The flag wasn't in the page itself, so I followed the relevant request and response and checked the headers too.

One of the **custom HTTP headers** contained the flag. It was easy to miss if I only looked at the URL or response body.

This was a good reminder that an HTTP exchange has more than just the page content. Headers can contain useful evidence too.

# Flag

```text
deadface{l3ts_get_Th3s3_fiL3$}
```

# Solved By

Kazikiri
