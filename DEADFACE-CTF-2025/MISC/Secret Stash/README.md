# Challenge Name

Secret Stash

# Author

@syyntax

# Description

Look for hidden files left by the novice DEADFACE user on the remote Linux host, and recover the flag.

# Solution

After getting into the Hostbusters machine, I looked for files that didn't show up in a normal directory listing.

Running `ls -a` showed the hidden entries, including a file called **`.dont_forget`**. I opened it and found the flag.

This was one of those simple Linux tricks that's easy to overlook when you're rushing: files starting with a dot can be hidden from a normal `ls`.

# Flag

```text
deadface{hostbusters2_4685d0c801939781}
```

# Solved By

Kazikiri
