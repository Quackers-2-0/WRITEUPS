# Challenge Name

Baby Exfil

# Author

0x157

# Description

Team K&K has identified suspicious network activity on their machine. 
Fearing that a competing team may be attempting to steal confidential data through underhanded means, they need your help analyzing the network logs to uncover the truth.

# Solution

Given a pcap to parse through the data, quite a lot of data but if you just go follow the tcp stream it'll be pretty easy. 
Most TCP are gibberish to human but some are human readable. Amongst them you'll get a python script that uses the key "G0G0Squ1d3Ncrypt10n" to XOR some files which you can find by going through the TCP stream, in the end you just need to find up POST that goes to /upload, they will be in hex bytes which you can extract the hex blobs and use the key found to decrypt the files and see the original file. 
One of the files contain the flag

# Flag

uoftctf{b4by_w1r3sh4rk_an4lys1s}

# Solved By

Kazikri
