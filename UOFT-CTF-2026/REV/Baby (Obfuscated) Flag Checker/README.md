# Challenge Name

Baby (Obfuscated) Flag Checker

# Author

SteakEnthusiast

# Description

A heavily obfuscated Python flag checker with a hint that full deobfuscation is unnecessary. The challenge asks you to recover a valid input for the checker; understanding the obfuscation could also help with a harder related challenge.

# Solution

This challenge gave me an obfuscated Python file, `baby.py`. The code was messy enough that I had trouble following it, so I used ChatGPT to help figure out what the checker was doing.

Instead of trying to clean up the whole program, we focused on the part that checks the input. The analysis found that the program expected **74 characters** and checked **nine chunks** of the input.

One suggested way to recover those chunks was to use Python's `sys.settrace()` to inspect the values being compared while the program runs. That would let us see the expected text without having to reverse every part of the obfuscated code. I don't have a log showing me running that tracer myself, so I'm treating it as the method we worked out during the analysis.

These were the pieces recovered in the chat:

```text
uoftctf{d1d_
y0u_m0nk3Y_
p4TcH_
d3BuG_
r3v_0r_
0n3_sh07_
th15_w17h_
4n_1LM_
XD???}
```

I also tried getting a reusable deobfuscator made for future challenges, but it didn't properly clean up the file. In the end, the shortcut of looking at the checks was much more useful for this challenge.

# Flag

```text
uoftctf{d1d_y0u_m0nk3Y_p4TcH_d3BuG_r3v_0r_0n3_sh07_th15_w17h_4n_1LM_XD???}
```

# Solved By

Kazikiri
