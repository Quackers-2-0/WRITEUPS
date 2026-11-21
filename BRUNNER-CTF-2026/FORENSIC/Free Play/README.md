# Challenge Name

Free Play

# Author

Quack

# Description

A worker had apparently spent weeks staring at the character roster in an old game. We were given a save file and a screenshot, and had to find what was hidden in the save.

# Solution

The screenshot made me think something was hidden in the image, possibly behind a password. But the challenge kept talking about the character roster, so the save file was worth checking too.

I tried `hexdump -C` on the save, but I couldn't really understand what I was looking at. With some help analyzing the file, we found strings that identified the game as **LEGO Star Wars: The Complete Saga**. That made the roster clue more interesting.

Most of the file looked like ordinary binary data. Around offset `0x9d20`, though, there was a long section with almost nothing but `00` and `03` bytes. Having only two repeated values made it look like someone was hiding binary data.

We treated `00` as `0` and `03` as `1`, then grouped the result into eight-bit chunks. Here's a small script that shows the idea:

```python
with open('SaveGame1', 'rb') as f:
    data = f.read()

segment = data[0x9d20:0x9db8]
bits = ''.join('0' if b == 0 else '1' for b in segment)
message = ''.join(chr(int(bits[i:i+8], 2)) for i in range(0, len(bits), 8))
print(message)
```

It decoded to `strong_force_in_you`. Putting it inside the flag format worked.

What I liked about this challenge is that I didn't have to reverse the whole save format. I just needed to find the section that looked out of place and work out what those two values meant.

# Flag

```text
brunner{strong_force_in_you}
```

# Solved By

Kazikiri
