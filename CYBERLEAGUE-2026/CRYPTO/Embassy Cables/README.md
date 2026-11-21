# Challenge Name

Embassy Cables

# Description

An embassy signing terminal let us request ECDSA signatures on messages we chose. The goal was to recover its private signing key and submit it before the session ended.

# Solution

The challenge came with `challenge.py`, so I started by reading how it made the signatures. The terminal used **ECDSA on secp256k1**, which normally relies on a fresh, unpredictable number called a **nonce** for every signature.

This part of the source looked wrong:

```python
ENTROPY_SOURCE_BITS = 256
HASH_TRUNCATION = 16
EFFECTIVE_BITS = ENTROPY_SOURCE_BITS - HASH_TRUNCATION
```

The nonce generator only asked for **240 bits** of randomness, even though ECDSA was working with values close to 256 bits. That meant the top 16 bits of each nonce were always zero.

At first that didn't sound like much. We still didn't know the nonce itself, and it wasn't reusing the same nonce over and over. But a little information leaking from *every* signature is enough to become a problem if you can collect several of them.

The ECDSA calculation is:

```text
s = k^-1 (h + r*d) mod n
```

Here `d` is the private key, `k` is the nonce, and `r`, `s` and `h` are values the terminal gives us. Rearranging it gives:

```text
k = (r * s^-1) * d + (h * s^-1)   mod n
```

So every signature gives us another equation involving the same private key, and we already know that its nonce is smaller than `2^240`.

I used ChatGPT to help understand how to turn those equations into a **Hidden Number Problem** and solve it with **LLL**, a lattice-reduction algorithm. The attack script preserved in the chat was designed to request **24 signatures** and use the remaining operation to submit the recovered private key.

That last part mattered because the terminal allowed **25 operations per session** and generated a new private key for each connection. Collecting signatures in one session and reconnecting to submit the key wouldn't work.

We got the real flag after applying the key-recovery approach. What made this challenge interesting was that nothing had to reuse a nonce: just making every nonce slightly too small was enough to break the signing key.

# Flag

```text
CYBERLEAGUE{3mb4ssy_c4bl3s_n0nc3_l34k_7d2f}
```

# Solved By

Kazikiri
