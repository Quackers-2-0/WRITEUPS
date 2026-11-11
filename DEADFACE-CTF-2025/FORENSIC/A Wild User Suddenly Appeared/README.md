# Challenge Name

A Wild User Suddenly Appeared

# Author

@syyntax

# Description

Using the captured traffic from the MyShare incident, identify the account DEADFACE created for persistence, including its first name and password.

# Solution

1. I continued examining the provided PCAP after the successful administrator login identified in the earlier challenge.
2. I located the subsequent HTTP request to `admin.php`, where the submitted form fields showed that a new account was being created.
3. The user-creation data exposed the first name `Dorla` and its password, which I combined into the required flag format.

# Flag

```text
deadface{Dorla_SuP3RS3cr3tD34DF4C3#}
```

# Solved By

Kazikiri
