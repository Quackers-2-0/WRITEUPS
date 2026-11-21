# Challenge Name

Compromised

# Author

@syyntax

# Description

Determine the username and password of the account compromised during the MyShare intrusion by analyzing the captured network traffic.

# Solution

I went back to the MyShare PCAP and filtered the traffic for **HTTP POST requests**. Since the challenge was asking which account had been compromised, login requests seemed like the best place to start.

The administrator login traffic exposed the username **`bsampsel`** and the password **`Sparkles2025!`**. I put them together in the flag format.

This showed why unprotected login traffic is such a problem: credentials can end up sitting in the packet capture for anyone who can inspect it.

# Flag

```text
deadface{bsampsel_Sparkles2025!}
```

# Solved By

Kazikiri
