# Challenge Name

CreepNet

# Author

@AstralByte

# Description

Network traffic shows no obvious command-and-control activity through standard channels. Investigate how DEADFACE communicated with its server and recover the hidden message.

# Solution

1. I inspected the provided PCAP and filtered for DNS activity.
2. I looked at failed DNS lookups and noticed query names containing data that resembled Base64.
3. I decoded the encoded portions and reassembled the recovered message into the flag.

# Flag

```text
deadface{Its_Imp0rtant-T0.c@tch-Everyd3t@il}
```

# Solved By

Kazikiri
