# Are You Pylingual

**Event:** PatriotCTF 2025  
**Category:** Rev

## What I did

This challenge came with a compiled Python `pyc` file. Rather than setting up a local decompiler, I uploaded it to **PyLingual** to turn it back into readable Python.

The decompiled code showed the formula used to hide the contents of an output file. Once I could see that calculation, I worked backward and reversed the formula to recover the original text.

The key lesson was that **obfuscation isn't the same as encryption**. Making code awkward to read won't protect a secret if the program still contains everything needed to reverse it.

## Flag

```text
pctf{obFusc4ti0n_i5n't_EncRypt1oN}
```

## Solved By

Kazikiri
