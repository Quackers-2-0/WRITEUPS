# Challenge Name

The Source

# Author

syyntax

# Description

A department at TGRI was getting lazy with sensitive files. Rather than use the company’s approved filesharing application, a member used an application called MyShare and forgot to configure it to run HTTPS.

DEADFACE found the site and were able to compromise it and steal several sensitive files. Based on DEADFACE’s past behavior, let’s assume that flags discovered may be used as DEADFACE passwords.

First thing’s first: what is the IP address of the DEADFACE attacker? Submit the flag as deadface{X.X.X.X}.

SHA256: 12345

# Solution

The provided file was opened using Wireshark. Reviewing the log entries revealed source IP addresses, with the first recorded log containing the correct IP address.

# Flag

deadface{134.199.202.160}

# Solved By

SniperKill258
