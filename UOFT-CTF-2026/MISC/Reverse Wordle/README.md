# Challenge Name

Reverse Wordle

# Author

janky

# Description

A player always uses the same starting guess in Wordle. Given the results of three games, recover that starting word and submit the SHA-256 hash of the **uppercase** word inside `uoftctf{...}`.

# Solution

This one gave the results of three Wordle games and asked me to find a starting word that would have produced those same results. Then I had to SHA-256 hash the **uppercase** word and put the hash inside `uoftctf{...}`.

The three known answers were `REBUT`, `CRASS`, and `FIERY`. I first tried to work it out by hand, but I kept getting confused by the yellow tiles. Yellow means the letter is in the answer, just not in that spot. I was treating some yellows as if the letter couldn't be in the starting word at all.

I switched to Python to test possible starting words against all three answers. These were the feedback patterns from my notes:

```python
constraints = [
    ('REBUT', 'bbyby'),
    ('CRASS', 'ybbbb'),
    ('FIERY', 'bbbyb'),
]
# b = black, y = yellow, g = green
```

The code checked green letters first, then yellow letters, making sure it didn't count the same letter twice. That matters when a word has repeated letters. After checking possible Wordle guesses, the candidate was `SQUIB`.

I actually rejected `SQUIB` at first because my flag submission failed. After going back over it, the word was fine. I had made mistakes with the hash and the flag prefix, including typing `uofctf` instead of `uoftctf` once.

This is the final hashing step:

```python
import hashlib

word = 'SQUIB'
digest = hashlib.sha256(word.encode('ascii')).hexdigest()
print(f'uoftctf{{{digest}}}')
```

Once I fixed the submission, it worked. A rejected flag doesn't always mean the actual solution is wrong; sometimes it's just the way you submitted it.

# Flag

```text
uoftctf{64b28ded00856c89688f8376f58af02dc941535cbb0b94ad758d2a77b2468646}
```

# Solved By

Kazikiri
