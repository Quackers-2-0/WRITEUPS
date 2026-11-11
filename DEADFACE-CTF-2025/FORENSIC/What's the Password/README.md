# Challenge Name

What's the Password

# Author

@Deputy_Doge

# Description

Analyze the suspicious files collected after the Lytton Labs incident, recover the first Trojan Echoes flag, and prepare the artifacts for further investigation.

# Solution

1. I encountered a password-protected ZIP archive in the supplied materials.
2. I used John the Ripper with the `rockyou.txt` wordlist to recover the archive password.
3. I opened the protected archive and read the extracted files to find the first flag.

My original notes identify the tool and wordlist but do not preserve the exact commands or password.

# Flag

```text
deadface{Hello_my_friend}
```

# Solved By

Kazikiri
