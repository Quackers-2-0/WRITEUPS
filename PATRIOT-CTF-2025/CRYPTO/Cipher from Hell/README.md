# Cipher from Hell

**Event:** PatriotCTF 2025  
**Category:** Crypto

## What I did

I was given a Python encryption script and its encrypted output. Since the script showed exactly how the message was scrambled, I worked backward from the operations instead of trying random ciphers.

The script was doing a strange conversion involving **base 3**, so the important part was understanding the order of its calculations. I made an inverse version of the algorithm to undo the transformation and turn the result back into readable bytes.

Once I got the reverse process right, it gave me the flag. The main trick was working backward from the code instead of guessing at the encryption.

## Flag

```text
pctf{a_l3ss_cr4zy_tr1tw1s3_op3r4ti0n_f37d4b}
```

## Solved By

Kazikiri
