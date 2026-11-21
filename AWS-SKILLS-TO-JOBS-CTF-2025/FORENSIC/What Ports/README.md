# What Ports

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Forensic

## What I did

The challenge asked which TCP ports were open on a Linux machine at **192.168.110.130**, using a supplied `server.pcap`.

In Wireshark, I looked for connections where the server replied with **SYN and ACK**, which generally means the port accepted a connection attempt:

```wireshark
tcp.flags.syn == 1 && tcp.flags.ack == 1
```

I also checked **RST** responses, which can show ports that refused a connection. Comparing the responses for the target machine helped me narrow the open ports down to **21, 5000 and 6969**.

The useful skill here was reading what the TCP handshake says about a host without needing to run a new port scan myself.

## Answer

```text
21, 5000, 6969
```

## Solved By

Kazikiri
