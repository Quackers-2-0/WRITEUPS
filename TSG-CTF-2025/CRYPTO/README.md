# Challenge Name

PtQagain

# Description

I’ll give you p + q encrypted twice with the same scheme! It’s the same thing anyway, so no problem... right?

Hints for beginners:

The result of running problem.py is stored in output.txt.
When you read problem.py, you can see that it encrypts the value FLAG and outputs it. Also, there are some other suspicious data in the output. Since the real value of FLAG is hidden, let's find it out from problem.py and its output!
The original FLAG is a string (more precisely, a byte string), but to encrypt it, it is converted to the corresponding integer m with the code m = bytes_to_long(FLAG). If you get m, then you can obtain the FLAG with the code long_to_bytes(m).

# Solution

Had to run a python script on RSA Conversion

````
from Crypto.Util.number import long_to_bytes, inverse
from math import gcd
N = 8415991046378822808678417368330656853369287762382257778863000234996179456832593823172163438175141897794450973258850755059008064965170892039119019048620633841637561375984016774249397631671121650489315360389403325290330278785155612107593553850666774120554971030956010302235061389242836031692105637093
e = 65537
c = 5574165375242524333573826610902926393700854735994229967966464169834696611810072076594787795367537651736635669504446868847168099681864093409935801174468222766808668365620150398151512438201199626223130807413078543403236814636420490358773639850910401283271784777683577389622203357416804633487506912900

find p or q
factor = None
for k in range(1, 2000):  # enough for 512-bit primes
    val = pow(10, k, N) - 1
    g = gcd(val, N)
    if 1 < g < N:
        factor = g
        print(f"[+] Found factor at k = {k}")
        break
if factor is None:
    raise Exception("Failed to factor N")
p = factor
q = N // p
print("[+] p and q recovered")

RSA Private Key
phi = (p - 1) * (q - 1)
d = inverse(e, phi)

Decrypt Key
m = pow(c, d, N)
flag = long_to_bytes(m)
print("\n[+] FLAG:")
print(flag.decode())
````

# Flag

TSGCTF{p+q_4nd_p+q_b3in9_th3_s4me_1s_0bv1ous_ri9h7?aZ3mQ9Lk7P2xB8R}

# Solved By

SniperKill258
