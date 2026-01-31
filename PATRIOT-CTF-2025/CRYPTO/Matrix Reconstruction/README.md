# Challenge Name

Among USniversity

# Author

DJ Strigel

# Description

Someone’s been messing with your secure communication channel… and they’ve left traces!

You’ve intercepted a mysterious ciphertext and a series of leaked internal states from a rogue pseudorandom generator. It seems the generator is powered by a secret 32×32 matrix A and an unknown 32-bit vector B.

Your mission: reverse-engineer the system! Use the leaked states to reconstruct the hidden matrix, uncover the XOR constant, and decrypt the message. Only then will the true flag reveal itself.

Remember: the keystream bytes come from the lowest byte of each internal state. Pay attention to the details.

# Solution

The challenge provided an encrypted file and leaked keystream states. The lowest byte of each state was extracted to reconstruct the keystream. XOR decryption of the ciphertext using the reconstructed keystream produced an ASCII plaintext containing the flag.

````
Step 1 - Open the cipher.txt 
with open(r"file_directory\cipher.txt", "rb") as f:
    ciphertext = f.read()

Step 2 - Leaked states 
states = [2694419740,2430555337,3055882924,228605358,4055459295,676741477,
1030306057,1320993926,2317712498,3680836913,1922319333,1836782265,
1490734773,218490631,4065897775,3125259028,189241330,1710684784,
2355890305,95797196,813001417,1021781706,3522243094,1603928614,
1122416469,4125638785,2423341845,3666529189,61609182,2391267942,
148130332,4246509548,3552866507,1487751530,1895017353,327726050,
4251037246,22647618,3958787364,227107204]

Step 3 - Build keystream from lowest byte of each state 
keystream_bytes = bytes([s & 0xff for s in states])

Step 4 - Decrypt by XOR
plaintext = bytes([c ^ keystream_bytes[i % len(keystream_bytes)] for i, c in
enumerate(ciphertext)])

Step 5 - Print decrypted text
try:
     print("Decrypted flag:", plaintext.decode('ascii'))
except UnicodeDecodeError:
     print("Decrypted bytes contain non-ASCII characters. Raw bytes:")
     print(plaintext)
````

# Flag

pctf{mAtr1x_r3construct?on_!s_fu4n}

# Solved By

SniperKill258
