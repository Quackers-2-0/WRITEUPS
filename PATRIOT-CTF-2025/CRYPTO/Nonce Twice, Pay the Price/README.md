# Nonce Twice, Pay the Price

**Event:** PatriotCTF 2025  
**Category:** Crypto

## What I did

This was one of the cryptography challenges where I understood the general mistake better than the full maths. The clue was that the signing process reused a **nonce**. That's a serious problem for ECDSA because reusing the same random value can expose information that should stay secret.

My old notes mention recovering values from the signing data, then doing an XOR-based decoding step to get the flag. I did manage to recover it, but I didn't save the calculations or final script.

The maths was the hardest part for me. I understood why repeating a nonce was dangerous, but the calculations were definitely something I wanted to study more.

## Flag

```text
pctf{ecdsa_n0nc3_r3us7e_get!s_y0u8_0wn1ed}
```

## Solved By

Kazikiri
