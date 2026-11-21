# Challenge Name

Let Me In

# Author

@syyntax

# Description

Use the supplied credentials to access a remote DEADFACE Linux machine and locate a flag in the user’s home directory.

# Solution

The challenge gave me credentials for a remote Linux machine. I used **SSH** to connect, then looked through the files in the account's home directory.

A text file there contained the flag, so this was a simple starting point for the Hostbusters series.

There wasn't any need for exploitation yet. The main job was to get connected and actually check what the account could access.

# Flag

```text
deadface{hostbusters1_cf6a12ddf781cfbc}
```

# Solved By

Kazikiri
