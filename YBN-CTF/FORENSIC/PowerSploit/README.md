# Challenge Name

PowerSploit

# Author

Not recorded in the recovered conversation.

# Description

Northland SOC raised an alert after a suspicious PowerShell script was downloaded onto an internal workstation. The challenge provided a network capture and a remote interactive quiz (`nc tcp.ybn.sg 18339`) asking eight questions about the incident.

# Solution

This was more of an investigation than an exploit. We had a PCAP showing suspicious PowerShell activity, and a remote quiz asked eight questions about what happened.

I used Wireshark, especially **Follow TCP Stream**, to see what the machine sent and received. Some answers were visible in the traffic. Others took more checking, especially the connection duration and the persistence details.

These are the answers I recovered:

| Question | Answer | What helped |
| --- | --- | --- |
| C2 server IP | `172.16.100.12` | Repeated traffic to the suspicious host |
| System info output file | `ciminfo.txt` | File references in the TCP stream |
| Persistence registry path | `HKCU\Software\Microsoft\Windows\CurrentVersion\Run` | PowerShell activity and the accepted quiz answer |
| Persistence entry name | `ServiceUpdater` | `Updater` was rejected, so I checked the name again |
| Reverse-shell port | `443` | The connection used for the shell; `4444` was wrong |
| Connection duration | `110.588415` seconds | Start and end packet times |
| Contents of the confidential file | `0d550b3f9c28988953197836d9f2db696db24fbb64d9dd10e7c6e4ba5003e51f` | Contents seen in the captured traffic |
| New admin username | `Ichinose` | Account-creation commands in the capture |

The timing question was especially annoying. It wanted **six decimal places**. Answers like `110`, `111`, and `113` were all rejected until I used `110.588415`.

I also found that the suspicious PowerShell wasn't just plain text. It had a Base64-encoded and compressed second stage, which was run through an obfuscated `IEX` command. I tried decoding it later, but I accidentally copied extra characters into the Base64 string, so that attempt failed.

After the eighth correct answer, the quiz gave me the flag. This was good practice for reading network traffic, following what an attacker did, and checking the small details instead of guessing.

# Flag

```text
YBN{802846803304586997887428e6c6ee86}
```

# Solved By

Kazikiri
