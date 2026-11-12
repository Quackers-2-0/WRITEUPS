# Challenge Name

Wat is this

# Description

The challenge gave me a huge, obfuscated JavaScript file called `wat-is-this.js` and an encrypted `secret.zip`. I had to work out how the program checked its input, then recover the password for the ZIP.

# Solution

The JavaScript file was over 2 MB and was pretty painful to read normally. Rather than untangle every line, I looked at what it **generated when it ran**. I used a small `Function`-constructor hook (`unwrap.js`) to save the generated code to `payload.js`. That made the next part easier to follow.

The payload was using **WebAssembly (WASM)** modules hidden inside its memory. I instrumented the runtime to dump parts of the heap and eventually separated out three modules:

- `module_0_494.wasm` — mixing and random-number functions.
- `module_494_1546.wasm` — most of the password checks.
- `module_1546_valid.wasm` — the final `check` function and password-output buffer.

At first I thought I might need to guess a password. Looking through the WASM checks showed something much better: the program expected a **48-byte input**, but only 21 of those bytes actually mattered. They were at positions `0..6`, `16..22`, and `32..38`. The other bytes could stay at zero.

The checks were built from 32-bit operations, including rotates, XORs and lookup tables. I rebuilt that logic in JavaScript and compared it against the real WASM functions. This checking step mattered: my first model got the way `derive()` carried its previous state wrong, and another check was reading bytes that `derive()` had just written rather than fixed constants.

Once the model matched, I used **Z3** to solve the 21 active bytes. My first solver version used lookup-table arrays and was much slower than it needed to be. Changing those lookups into bit-vector `If` selections made the problem manageable.

The main saved solver is `solve_with_z3.js`. The saved log shows:

```text
solver_check_done sat
wasm_recheck 1
artifacts_written
```

So the solver found an answer, and the original WASM checker accepted it too. The recovered 48-byte input was:

```text
31306d266139370000000000000000006834735f6631340000000000000000006f213230764033000000000000000000
```

The meaningful parts read as `10m&a97`, `h4s_f14` and `o!20v@3`. Those were **not** the ZIP password yet. Once the WASM check passed, it produced this 32-byte password:

```text
oh_y34h,_!ts_a11_c0ming_to93t#er
```

I used that password to open `secret.zip`. The extracted `flag` file is still in my old folder, and it contains the flag below.

This one took a lot of reverse engineering and solver work. Codex helped with the analysis, but what really made it work was narrowing the problem down to 21 bytes and checking the reconstructed logic against the actual WASM instead of assuming the model was correct.

# Flag

```text
TISCDCSG{the_f1ag_ch0sen_speci4lly_for_th3_wasm}
```

# Solved By

Kazikiri
