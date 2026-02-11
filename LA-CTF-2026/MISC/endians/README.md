# Challenge Name

endians

# Author

pstorm

# Description

I was reading about Unicode character encodings until one day, my flag turned into Japanese! Does little- endian mean the little byte's at the end or that the characters start with the little byte?

# Solution

A text file chall.txt containing seemingly random Japanese characters, 氀愀挀琀昀笀㄀开猀甀爀㌀开栀　瀀攀开琀栀㄀猀开搀　攀猀开渀　琀开最㌀琀开氀　猀琀开㄀渀开琀爀愀渀猀氀愀琀椀　渀℀紀. 
The python snippet they provide gave a hint at encoding/decoding and also a hint in the description little-endian bytes. 
Encode it back as UTF-16 Big Endian bytes. Decode it as UTF-16 Little Endian to recover the original flag.

````
with open(r"file_directory\chall.txt", "r", encoding="utf-8") as f:
    weird_text = f.read()

flag = weird_text.encode("utf-16be").decode("utf-16le")
print(flag)
````

# Flag

lactf{1_sur3_h0pe_th1s_d0es_n0t_g3t_l0st_1n_translati0n!}

# Solved By

SniperKill258
