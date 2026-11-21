# Hiding in the s64dows

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Steg

## What I did

I was given a text file that looked like Base64. Decoding it once only gave me another Base64-looking string, so I kept peeling the layers. My notes recorded **52 layers** in total.

The catch was that the actual flag wasn't in the decoded text. It was hidden in the **unused bits near the Base64 padding**. Those bits don't normally change the decoded bytes, which makes them a handy place to hide information.

I asked ChatGPT to help me write a Python script, `brutebits_extract.py`, that decoded each layer and collected the unused bits from the last Base64 character. The script then tried different bit offsets and byte orders to turn them back into readable text.

That recovered the flag. This challenge was a good reminder to look at how data is **encoded**, not only what it decodes to.

## Flag

```text
flag{p4aDD3d}
```

## Solved By

Kazikiri
