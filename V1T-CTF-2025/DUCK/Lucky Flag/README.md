# Lucky Flag

**Event:** V1t CTF 2025  
**Category:** Duck

## What I did

This one was a small web challenge built around luck. The page's script picked a random number from **0 to 1000**, and you needed it to land on **36**.

You could keep trying until the right number came up, but there was a much easier route. Looking at the page source showed the script and a piece of **Base64-encoded text**. Decoding it revealed the flag without having to sit there waiting for the random number.

It was a good reminder to inspect the client-side code before assuming a web challenge needs lots of repeated attempts.

## Flag

```text
v1t{lucky_ducky}
```

## Solved By

Kazikiri
