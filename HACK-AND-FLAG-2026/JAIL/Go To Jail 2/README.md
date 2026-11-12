# Challenge Name

Go To Jail 2

# Description

The service let me execute JavaScript but blocked a long list of dangerous words, including `constructor`, `process`, `fs`, `flag`, and `read`. I had to reach a file despite the filter.

# Solution

The blacklist looked annoying, but the JavaScript interpreter still understood expressions that built the forbidden words at runtime. Instead of typing `constructor` directly, for example, I could make it from `'con'+'structor'`.

I connected to the challenge service with `nc` and tried reaching Node.js through a function's constructor. My first test listed files, but I didn't see any output because I hadn't printed the result. Adding `console.log()` made it show the filesystem listing.

The working idea was to get a reference to the Node.js process, load its built-in filesystem module, and read the flag. The payload was:

```javascript
[].filter['con'+'structor']('return pro'+'cess')()
  .getBuiltinModule('f'+'s')['re'+'adFileSync']('fl'+'ag.txt', 'utf8')
```

The empty array's `filter` function gives access to the `Function` constructor. That runs `return process`, and the rest of the expression gets the filesystem module and reads `flag.txt`.

Splitting words across string pieces avoids putting the blocked text directly in the input. The filter checked what I typed, not what those strings became when JavaScript ran them.

Once I used the file-read payload, I got the flag. The main lesson was that **blocking a few strings is not a safe way to sandbox JavaScript**.

# Flag

```text
HNF26{j41L5_s!t_ar0und_campf1r3s_4nd_t3ll_sc4ry_st0ri3s_ab0ut_u}
```

# Solved By

Kazikiri
