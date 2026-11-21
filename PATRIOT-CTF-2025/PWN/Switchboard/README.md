# Switchboard

**Event:** PatriotCTF 2025  
**Category:** Pwn

## What I did

This one had a more complicated intended theme involving kernel exploitation, but my actual solve was much simpler.

After unpacking the supplied ZIP, I looked through the extracted files and searched for the flag format. The flag was already present in the material, so I didn't need to build or run a kernel exploit.

For a search like that, the approach is basically:

```bash
grep -Rni 'pctf{' .
```

It was a good reminder to check the files I was given before spending ages on the hardest-looking route.

## Flag

```text
pctf{n1c3_k3rn3l_sl4b_938f238}
```

## Solved By

Kazikiri
