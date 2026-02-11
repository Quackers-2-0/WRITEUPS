# Challenge Name

ooo

# Author

k

# Description

Surely everyone knows the difference between the Cyrillic small letter o, the Greek small letter omicron, and the Latin small letter o, right? :)

# Solution

o from different Unicode scripts (Cyrillic, Greek, Armenian) Where vals is the array provided in the script. 
Recover the flag: Flags start with lactf{, so start with 'l' and iterate. 
Made a python script to solve this challenge.

````
vals = [
    205, 196, 215, 218, 225, 226, 1189, 2045, 2372,
    9300, 8304, 660, 8243, 16057, 16113, 16057,
    16004, 16007, 16006, 8561, 805, 346, 195,
    201, 154, 146, 223
]

# flags start with lactf{
flag = [ord('l')]

for i in range(len(vals) - 1):
    flag.append(vals[i] - flag[i])

print("Flag:", ''.join(map(chr, flag)))
````

# Flag

lactf{gоοօỏơóὀόὸὁὃὄὂȯöd_j0b}

# Solved By

SniperKill258
