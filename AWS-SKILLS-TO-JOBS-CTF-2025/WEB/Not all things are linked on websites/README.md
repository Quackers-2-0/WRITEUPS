# Not all things are linked on websites

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Web

## What I did

The site didn't link to every page or folder it had. I used **DirBuster** with a wordlist from Kali to look for directories that weren't visible through the normal website navigation.

The scan found a directory named `/security`. Opening that path revealed the answer. The main lesson was that a hidden URL isn't protected just because the homepage doesn't link to it.

The useful lesson was that a directory can still be found by scanning even if nobody links to it directly.

## Answer

```text
verySecUReDireCToRY_1180018
```

## Solved By

Kazikiri
