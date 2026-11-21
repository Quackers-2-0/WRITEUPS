# Trust Vault

**Event:** PatriotCTF 2025  
**Category:** Web

## What I did

I spent a while trying SQL injection on the site's **bookmarks page**, but nothing worked. It turned out I was testing the wrong place: the bookmarks page cleaned up the input before using it.

Looking through the page source, I found a comment pointing to a hidden route:

```text
/search
```

The input handling on that route was different, and SQL injection worked there. Once I stopped attacking the protected form and tried the search endpoint, I could get the flag.

The biggest lesson was to check the application's other routes instead of assuming the obvious input box was the only place worth testing.

## Flag

```text
PCTF{SQL1_C4n_b3_U53D_3Ff1C13N7lY}
```

## Solved By

Kazikiri
