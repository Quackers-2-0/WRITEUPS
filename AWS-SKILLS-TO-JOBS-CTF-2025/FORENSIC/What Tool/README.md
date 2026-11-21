# What Tool

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Forensic

## What I did

I looked through the provided `server.pcap` in Wireshark to figure out which tool had been downloaded or used.

Filtering for **HTTP** traffic helped narrow it down. I checked the GET requests and the downloaded file details until I found the tool name: `nmap`.

It was a useful reminder that even when you don't have access to the machine itself, a packet capture can sometimes show what tools someone transferred over the network.

## Answer

```text
nmap
```

## Solved By

Kazikiri
