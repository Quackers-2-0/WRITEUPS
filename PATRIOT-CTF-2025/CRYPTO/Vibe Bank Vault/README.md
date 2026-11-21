# Vibe Bank Vault

**Event:** PatriotCTF 2025  
**Category:** Crypto

## What I did

This challenge had **five layers** to work through. Some of the later layers were simple calculations, but another layer was annoying because of how it encoded emojis.

The biggest problem was the first layer, which involved a custom bcrypt-like system. Ordinary cracking scripts weren't working as expected. I worked out that the challenge's key generation could produce roughly **3,844 possible keys**, but checking all of them was still slow, even after trying to improve the brute-force approach.

I eventually got through the layers and recovered the flag. Most of my time went into the first layer, not the simpler calculations at the end.

## Flag

```text
PCTF{g00d_v1b3s_b4d_3ntropy_sync72_b4ck1ng}
```

## Solved By

Kazikiri
