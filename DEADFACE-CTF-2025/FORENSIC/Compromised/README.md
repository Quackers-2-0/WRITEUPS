# Challenge Name

Compromised

# Author

@syyntax

# Description

Determine the username and password of the account compromised during the MyShare intrusion by analyzing the captured network traffic.

# Solution

1. I filtered the PCAP for HTTP requests using the `POST` method.
2. I focused on the authentication requests associated with the administrator page.
3. The relevant request revealed the compromised username `bsampsel` and password, giving the flag.

# Flag

```text
deadface{bsampsel_Sparkles2025!}
```

# Solved By

Kazikiri
