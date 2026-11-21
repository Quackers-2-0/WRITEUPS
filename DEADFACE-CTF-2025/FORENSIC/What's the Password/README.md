# Challenge Name

What's the Password

# Author

@Deputy_Doge

# Description

Analyze the suspicious files collected after the Lytton Labs incident, recover the first Trojan Echoes flag, and prepare the artifacts for further investigation.

# Solution

The supplied files included a **password-protected ZIP**, so I couldn't inspect the contents straight away.

I used **John the Ripper** with the `rockyou.txt` wordlist to recover the archive password. After opening the ZIP, I read through the extracted files and found the first flag in the Trojan Echoes series.

This was a good reminder to try a sensible wordlist before assuming a protected archive will need some complicated attack.

# Flag

```text
deadface{Hello_my_friend}
```

# Solved By

Kazikiri
