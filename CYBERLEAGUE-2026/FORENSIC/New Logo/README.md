# Challenge Name

New Logo

# Description

We were given `3_ffox_main_process.raw` and told to recover a racing team's unfinished logo from memory. The challenge also said every letter in the flag was uppercase.

# Solution

I thought the `.raw` file might be a full memory dump, so the first thing I checked was what it actually contained:

```bash
file 3_ffox_main_process.raw
```

It turned out to be an **ELF core dump of a Firefox process**, not a whole operating system's RAM. That changed the way I approached it. I didn't need to rebuild a process list; I needed to find whatever Firefox had kept in memory.

I searched the dump for links, image names and anything related to the logo:

```bash
strings -a -n 5 3_ffox_main_process.raw \
  | grep -Ei 'https?://|png|jpe?g|svg|logo|race'
```

The big clue was **FlipAnim**, a browser-based drawing tool. The dump also contained references to its `flipanim21` IndexedDB database and a `frames` store.

I tried the more obvious approach of carving PNGs and other image files, but the valid images I found were just browser assets and unrelated pictures. That made sense in hindsight: an unfinished drawing can be stored as **brush strokes and coordinates**, not as a finished PNG.

The useful frame was saved as a JSON-like record with fields such as `curr`, `data`, `color`, `size`, `x`, `y` and `points`. There were **42 drawing instructions**. The awkward part was that Firefox had kept a readable copy in **UTF-16LE**, so a normal ASCII string search could miss it.

Searching for little-endian text was much more useful:

```bash
strings -a -e l -n 10 3_ffox_main_process.raw \
  | grep -Ei 'flipanim|"curr"|"type":"dc"'
```

The saved analysis found the frame at offset `0xa7fe5428` (`2818462760` in decimal). After decoding the UTF-16LE text and removing the extra quotation marks around it, I had a JSON record with all the strokes.

I used `solve_new_logo.py` to turn those instructions back into a drawing. The script starts each stroke at its saved `(x, y)` position, adds the relative point movements, and builds the same kind of curves FlipAnim uses. It produced a recovered SVG and an image of the logo.

The recovered drawing spells out four lines. Some of the round characters were easy to mix up, but the correct flag uses **zeroes** in `C00L` and `BR0`:

```text
CYBERLEAGUE{
C00L_
LOGO_
BR0_
ZZZ}
```

Putting those together gives the flag below. The original core dump was several gigabytes, so it isn't included with this writeup. The recovered image and script are saved in my local challenge files and can be added separately.

# Flag

```text
CYBERLEAGUE{C00L_LOGO_BR0_ZZZ}
```

# Solved By

Kazikiri
