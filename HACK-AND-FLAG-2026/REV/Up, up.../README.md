# Challenge Name

Up, up...

# Description

The challenge gave me an unlabeled Game Boy ROM, `game.gb`. I thought the cartridge header might be broken, so I started by checking it with `rgbfix`.

# Solution

The challenge described a cartridge with no label, so I first tried:

```bash
rgbfix -v game.gb
```

It didn't print anything useful. The ROM checks turned out to be valid, and the internal game title was empty on purpose. The missing label was more of a distraction than the actual problem.

Looking at the ROM data gave a much better clue. There was a ten-byte sequence near offset `0x22c`:

```bash
xxd -s 0x22c -l 10 game.gb
```

```text
40 40 80 80 20 10 20 10 02 01
```

That looked like button inputs. Using the button mapping found during the analysis, the sequence matched:

```text
UP UP DOWN DOWN LEFT RIGHT LEFT RIGHT B A
```

That's the **Konami code**. The ROM had a hidden screen connected to that sequence, and its message was stored as Game Boy graphics rather than ordinary text. The graphics analysis showed the flag, and I later found a screenshot of the result to confirm the exact spelling. The screenshot is clearer than the text recovered from the old chat.

This challenge ended up being about recognizing a hidden input sequence, not repairing a damaged cartridge.

# Flag

```text
HNF26{m3d1um_b4ll_kn0wl3dge}
```

# Solved By

Kazikiri
