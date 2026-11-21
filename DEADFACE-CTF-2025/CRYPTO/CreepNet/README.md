# Challenge Name

CreepNet

# Author

@AstralByte

# Description

Network traffic shows no obvious command-and-control activity through standard channels. Investigate how DEADFACE communicated with its server and recover the hidden message.

# Solution

I started with the PCAP and looked at the DNS traffic. Nothing obvious was jumping out from normal web connections, so I paid more attention to the **failed DNS lookups**.

Some of the requested domain names had pieces that looked like **Base64** instead of normal hostnames. I decoded the encoded parts and put them back together. That gave me the hidden message and the flag.

The interesting part was that the traffic looked like DNS noise at first. The data was hidden in the names being queried, not in a normal HTTP download.

# Flag

```text
deadface{Its_Imp0rtant-T0.c@tch-Everyd3t@il}
```

# Solved By

Kazikiri
