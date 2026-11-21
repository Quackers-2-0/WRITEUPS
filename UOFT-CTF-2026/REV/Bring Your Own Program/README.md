# Challenge Name

Bring Your Own Program

# Author

SteakEnthusiast

# Description

Team K&K discovered an emulator for an unfamiliar architecture. The provided JavaScript emulator accepts hex-encoded bytecode; the challenge is to make that program read a protected flag file despite capability restrictions.

# Solution

This one confused me at first. The challenge came with `chal.js`, which looked like an emulator, and a file called `n` containing `8`. Connecting to the server didn't show a menu. When I typed `ls`, it just replied `ERR`, so I knew it wasn't a normal shell.

I used ChatGPT to help read through the JavaScript. It turned out the server wanted a **program written as a hex string** for its own little virtual machine.

The VM had a `caps` object that controlled which functions a program could use. Key `10` gave access to a safe file-read function, but key `0` could read a file using an absolute path, including `/flag.txt`. The program wasn't supposed to let us use key `0` directly.

The bug was in an **inline cache**. To make lookups faster, the VM remembered where a function was stored. But when the program changed the scope, the order of the stored functions could change while the cache still pointed to the old position.

The payload used that mistake by first looking up the allowed key `10`, changing the scope, then running the same lookup again. This time the cached position pointed to key `0`, so the program could read `/flag.txt`.

ChatGPT helped work out this payload:

```text
0803020400636170730209002f666c61672e74787401000000000000f03f0200002101060003210206010a6107090070010a01070260eeff0104013005020401043105
```

I sent it to the service:

```bash
nc 35.245.96.82 5000
```

The server returned the flag. I didn't find the cache bug on my own, but getting the payload working helped me understand how a small caching mistake could bypass the VM's restrictions.

# Flag

```text
uoftctf{c4ch3_m3_1n11n3_h0w_80u7_d4h??}
```

# Solved By

Kazikiri
