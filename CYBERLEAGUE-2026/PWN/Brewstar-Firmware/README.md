# Challenge Name

Brewstar-Firmware

# Description

The BrewStar coffee machine had a recipe manager and an exposed debugging console. Its roast-analysis feature wasn't checking how much data it wrote into a recipe.

# Solution

The bug was in **Recipe Manager → Adjust grind settings → Import roast analysis**. Each recipe was allocated with `malloc(0x78)`, but the program could read `0x68` bytes starting **0x20 bytes into that recipe**:

```c
read(0, recipe->profile, 0x68);
```

That means the input could go **16 bytes past the allocation** and overwrite the following heap chunk's metadata.

The exploit was a heap chain rather than a simple jump to a win function. The saved `solve_brewstar_fixed.py` script shows how it worked:

First, it put several recipe chunks beside each other, then changed the following chunk's size to make it look much bigger. Freeing and reallocating that fake chunk exposed an **unsorted-bin pointer**, which let the solver work out where libc was loaded.

The next trick was to make two recipe slots point to the same heap memory. Freeing it through one slot and reading it through the other leaked glibc's **safe-linking value**. With that value, the solver could use **tcache poisoning** to move an allocation near `environ`, leak a stack address, and eventually reach `main`'s saved return address.

The final chain was basically:

```text
ret → pop rdi; ret → '/bin/sh' → system
```

It was carefully positioned to avoid overwriting the stack canary. Exiting the program returned through the overwritten address and gave a shell.

But my **first solver didn't work**. I ran it on Kali and ended up pressing Ctrl+C because it was stuck waiting for a `recipe> ` prompt. The problem wasn't the heap exploit: the script had already consumed the prompt at the end of one helper function, then tried to wait for the same prompt again at the start of the next helper.

The fix was to synchronize with the recipe menu once, then send later menu choices directly. After that change, I ran the fixed solver and got the flag.

This challenge taught me two things: how a small heap overflow can lead all the way to a ROP chain, and how an exploit script can fail just because its input/output handling is wrong.

# Flag

```text
CYBERLEAGUE{f1rmw4r3_upd4t3_g0n3_wr0ng}
```

# Solved By

Kazikiri
