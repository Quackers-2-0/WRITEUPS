# Challenge Name

Brunner Radio

# Description

The challenge gave me a Python broadcast script and its combined output. Nine different transmissions had been mixed together, and one of them contained a `brunner{...}` flag. The question was which frequency carried the real message.

# Solution

The whole radio theme threw me off at first. I expected something involving actual radio signals, but looking at `broadcast.py` showed that it was really mixing **bits** together.

Each of the nine messages was 36 characters long. The script turned those characters into bits, then repeated each bit at a different interval. Frequency 1 appeared at every position, frequency 2 at every second position, frequency 3 at every third position, and so on. The output was the **sum** of those bits.

Once that made sense, the first nine samples of each row were enough to separate the messages. For example, sample 1 only contains frequency 1. Sample 2 contains frequencies 1 and 2, so subtracting the first result gives frequency 2. We keep doing this, removing anything we've already worked out.

I asked ChatGPT to help explain the math and turn it into Python. This is the main part of the solver:

```python
with open('total_broadcast.txt') as f:
    rows = f.read().splitlines()

recovered = [[] for _ in range(9)]

for row in rows:
    samples = [None] + list(map(int, row[:9]))
    bits = [None] + [0] * 9

    for frequency in range(1, 10):
        earlier = [d for d in range(1, frequency) if frequency % d == 0]
        bits[frequency] = samples[frequency] - sum(bits[d] for d in earlier)
        recovered[frequency - 1].append(bits[frequency])

def decode(bits):
    return ''.join(chr(int(''.join(map(str, bits[i:i+8])), 2))
                   for i in range(0, len(bits), 8))

for i, bits in enumerate(recovered, 1):
    print(i, decode(bits))
```

Some of the recovered messages had fake-looking prefixes such as `brummer{...}`. **Frequency 8** was the one with the real `brunner{...}` flag.

I got the flag, although I'll admit I still needed to go back over the math afterward. The nice part was learning that I didn't need to brute-force anything; the way the script combined the signals also gave us a way to separate them.

# Flag

```text
brunner{Brunsviger_is_in_the_air_<3}
```

# Solved By

Kazikiri
