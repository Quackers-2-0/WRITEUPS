# Photo Gallery

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Web

## What I did

The challenge had a simple photo-gallery website. I tried adding `/upload` to the site's URL and found that it exposed a directory containing uploaded files.

One of those files was named `flag.txt`. Opening it gave me the flag.

This was a quick example of why you shouldn't leave a file directory accessible just because the main website doesn't show its contents.

## Flag

```text
ctf{just_vomit_Gallery_content_}
```

## Solved By

Kazikiri
