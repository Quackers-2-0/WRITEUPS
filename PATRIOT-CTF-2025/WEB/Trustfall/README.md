# Trustfall

**Event:** PatriotCTF 2025  
**Category:** Web

## What I did

I found an API route that returned user information:

```text
/api/users
```

The site used numbered IDs for its records, so I tried asking for a specific user by adding the ID to the URL, such as `/api/users/1` or `/api/users/2`.

Then I tried **user ID 0**, and that was where the flag was hiding. The API was letting me view data just by changing the number, without properly checking whether I should have access to that user.

I originally called this directory traversal in my notes, but it's a better example of **broken object-level authorization (IDOR)**. I wasn't using `../` to move through directories; I was changing an object ID.

## Flag

```text
PCTF{authz_misconfig_owns_u}
```

## Solved By

Kazikiri
