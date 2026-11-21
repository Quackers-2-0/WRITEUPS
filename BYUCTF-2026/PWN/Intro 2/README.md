# Challenge Name

Intro 2

# Description

This BYUCTF challenge gave us a small Linux binary in the PWN category. It looked like a simple reverse-engineering check at first, but there was another bug hiding in how it printed our input.

# Solution

My first look at the binary found a function called `flag_check()` using `memfrob()`. That function XORs bytes with `0x2a`, so it was possible to decode what looked like an answer:

```text
byuctf{welcome_to_rev_fellas}
```

But something felt wrong. Why would a **PWN** challenge have a flag about reverse engineering? I pointed that out, and we went back to the binary instead of stopping there.

The real weakness was that the program passed our input directly to `printf()` as a **format string**. It also used **PIE**, so the addresses changed each time it ran, but the binary had **Partial RELRO**, leaving the `printf` entry in the GOT writable.

The idea was to leak a code address with `%p`, calculate the PIE base, then use `%hn` to change `printf@GOT` so that the next call to `printf()` went to the hidden `win()` function instead.

It took a few tries to get the script working. The first problems were pretty ordinary:

- I tried `nc chals.cyberjousting.com:1367`, but `nc` needed the host and port as separate arguments.
- The first script used `%37$p` to leak an address, but the result started with `0x7ffd...`, which was a **stack address**, not the PIE pointer we needed.
- We scanned the stack and found that `%39$p` was the useful PIE leak.
- Another helper script wrongly said the appended-address argument base was `13`. The marker was actually at `%28$p`, making the correct base `20`.

The patched solver used:

```python
LEAK_INDEX = 39
ARG_BASE = 20

MAIN_OFF = 0x5c9
WIN_OFF = 0x53c
PRINTF_GOT_OFF = 0x3030
```

It split the new address into three **two-byte `%hn` writes**, pointed `printf@GOT` at `win()`, and used the next `printf()` call to reach a shell. The final command read `flag.txt`.

I kept the old failed outputs and the final `intro_solve.py` script. They show why each change was needed, and the final script matches the last version in the old conversation. After patching it, I got the real PWN flag.

What I learned was not to trust the first suspicious-looking function in a binary, especially when its result doesn't match the challenge. I also got more practice with PIE leaks, GOT overwrites and debugging pwntools scripts.

# Flag

```text
byuctf{%p_yourself}
```

# Solved By

Kazikiri
