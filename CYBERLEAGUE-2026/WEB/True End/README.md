# Challenge Name

True End

# Description

This web challenge used a story system where you upload `.shard` files to continue a route. The goal was to reach the hidden `TRUE_END` scene and get the flag. We had the service handout and a sample shard to study.

# Solution

This one wasn't a normal login bypass or a simple SQL injection. The trick was that **different parts of the website read the same file differently**.

The handout showed a gateway, a preview service, a Python importer and a runtime. The gateway checked uploaded shards before the importer and runtime used them. If those programs disagreed about what a shard meant, I could potentially get the gateway to approve something the runtime treated as a hidden route.

The shard format was **CBOR**, a binary format a bit like JSON. The sample file had a very suspicious map: two keys that were different in CBOR but could become the same key in Python.

```text
CBOR true  = F5
CBOR 1.0   = F9 3C 00
```

In Python, `True == 1.0` is true, and they also have the same hash. When the importer used `cbor2` to turn that map into a Python dictionary, the second entry could replace the first. The gateway saw the **public Base route**, but the importer ended up using the **hidden Studio route**.

The next problem was the archive inside the shard. It wasn't one simple gzip stream. The gateway expected **three gzip members** containing `1`, `3` and `3` story records. The hidden route also changed how the importer ordered those members:

```text
Stored in the file: A, B, C
Read by the importer: A, C, B
```

I arranged the records so that the importer's order gave it the right six story cues followed by a `resume` for `route-12`. That reached a checkpoint and made the real service return a **signed receipt**.

That receipt was important because I couldn't just make one up. It contained an Ed25519 signature, so I needed the application to sign a real checkpoint for me.

The final shard used that receipt and another carefully arranged three-member gzip archive. It also needed two **CRC32 values** to match targets calculated from the receipt and the hidden memory sequence.

Brute-forcing the CRC32 values would have been a waste of time. CRC32 is linear over bits, so the solver could flip 32 chosen `0`/`1` positions and solve a small system of equations to reach each target. The saved `solve_true_end.py` handles that part.

The full solve used four requests:

```text
POST /api/routes/seal       (checkpoint shard)
POST /api/routes/continue   (get signed receipt)
POST /api/routes/seal       (final shard with receipt)
POST /api/routes/continue   (reach TRUE_END)
```

The working notes record this response:

```json
{
  "status": "TRUE_END",
  "scene": "ending-7221d42ffb151214",
  "line": "CYBERLEAGUE{TRU3_3ND1NG_UNL0CK3D_1_W0N_4T_L1F3}"
}
```

I had a few dead ends along the way: putting everything in one gzip member, trying a fake receipt, and arranging the members in the logical order instead of the order the importer expected. The main lesson was that **checking a file with one parser doesn't help if a different parser later makes the security decision**.

# Flag

```text
CYBERLEAGUE{TRU3_3ND1NG_UNL0CK3D_1_W0N_4T_L1F3}
```

# Solved By

Kazikiri
