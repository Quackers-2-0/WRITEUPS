# Trail Trace

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Forensic

## What I did

The challenge gave me a packet capture with **GPS data** inside the logs. The useful records used the **GPGGA** format, which contains location information from GPS receivers.

I extracted the coordinates from the relevant messages and used ChatGPT to help list them in order. Then I had a **KML file** made from those coordinates and opened it in Google Earth so I could see the path visually.

Following the path revealed the flag. This one taught me that packet captures can hold all kinds of application data, not just the usual web requests or suspicious network connections.

## Flag

```text
flag{y0u_f0und_m3}
```

## Solved By

Kazikiri
