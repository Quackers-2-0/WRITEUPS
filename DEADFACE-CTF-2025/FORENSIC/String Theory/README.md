# Challenge Name

String Theory

# Author

@Deputy_Doge

# Description

Using the artifacts recovered earlier in the Trojan Echoes series, find the next embedded flag, identified as `flag 01`.

# Solution

I had a recovered executable called `sample_01E9.exe.exe` from the earlier **Trojan Echoes** challenge. Rather than trying to run it, I checked the readable strings inside it.

I paged through the output and looked for something matching the flag format. That led to **`deadface{We_meet_again}`**.

My original notes include a command named `laystrings`, but I can't confirm whether that was an alias or just a typo. The important part of my solve was inspecting strings in the executable, not executing the suspicious file.

# Flag

```text
deadface{We_meet_again}
```

# Solved By

Kazikiri
