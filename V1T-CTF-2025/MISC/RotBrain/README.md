# RotBrain

**Event:** V1t CTF 2025  
**Category:** Misc

## What I did

The supplied file didn't give me readable text straight away, so I started looking at its bytes instead.

My notes say the data had been expanded into a UTF-8-like byte pattern. I had to **collapse the repeated byte pairs** and then reverse the order of the remaining sequence. Doing those steps in the right order gave me the original message and the flag.

This was more about understanding how the file's bytes had been rearranged than trying a standard ROT cipher.

## Flag

```text
v1t{r3v_1mg_4ge}
```

## Solved By

Kazikiri
