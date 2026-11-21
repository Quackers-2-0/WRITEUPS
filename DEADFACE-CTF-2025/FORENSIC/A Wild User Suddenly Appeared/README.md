# Challenge Name

A Wild User Suddenly Appeared

# Author

@syyntax

# Description

Using the captured traffic from the MyShare incident, identify the account DEADFACE created for persistence, including its first name and password.

# Solution

This was part of the same **MyShare** packet capture as the earlier challenges. After finding the compromised administrator login, I kept following the HTTP traffic to see what happened next.

One request to `admin.php` showed someone submitting a form to create another account. The fields exposed the new user's first name, **Dorla**, and the password used for the account.

Putting those two values into the required flag format gave me the answer. The nice part about this set of challenges was that each flag helped me understand a little more of the attack timeline.

# Flag

```text
deadface{Dorla_SuP3RS3cr3tD34DF4C3#}
```

# Solved By

Kazikiri
