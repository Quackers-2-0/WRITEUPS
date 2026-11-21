# Challenge Name

DISKLOCK

# Description

A fake old floppy-disk utility left behind a locked file called `PHOTO.LCK`, plus a log and manual. The manual made it sound like brute-forcing the password would take hundreds of thousands of years.

# Solution

I started with the simple tools: `file`, `strings`, and `xxd`. The locked file didn't identify itself as anything, but `strings PHOTO.LCK` kept showing pieces like `91FL0PPY`. I guessed that was probably the password or encryption key.

The manual and hex dump pointed toward **repeating-key XOR**, which is much weaker than the scary password warning made it sound. A repeated key leaves patterns in the data, and a known file header can help us work out the key's alignment.

The important correction was that the bytes were lining up with `FL0PPY91`, not `91FL0PPY`. With that key, XORing the start of the file gave the `BM` header of a BMP image:

```text
42 4d ...   # BMP file signature
```

I asked whether I could do the rest in **CyberChef** instead of using a script. The recipe was just:

```text
Input: PHOTO.LCK (raw file)
Operation: XOR
Key: FL0PPY91 (UTF-8)
Output: save as PHOTO.BMP
```

That recovered the image. It showed an old DOS-style screen displaying a `NOTE.TXT` file, with the flag written in yellow text. I found the screenshot again later, so I could finally add the exact flag here.

The nice part was learning that a big password space means very little if the encryption method itself leaks the key.

# Flag

```text
HNF26{wh0_n33ds_4_k3y_wh3n_y0u_h4v3_4_h34d3r}
```

# Solved By

Kazikiri
