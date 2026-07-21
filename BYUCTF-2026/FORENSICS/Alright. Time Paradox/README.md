# Challenge Name

Alright. Time Paradox

# Author

JC6143

# Description

To maintain a constant testing cycle, I simulate daylight at
all hours and add adrenal vapor to your oxygen supply. So
you may be confused about the passage of time. The point
is, yesterday was your birthday. I thought you'd want to
know.

*The pcap file contains a flag for each of the following challenges: "There will be cake", "Are you still there?", "Alright. Paradox time", and "Corrupted Cores".

If the flag you found doesn't work, then it most likely belongs to one of the other 3 challenges.

Hint: What protocol is associated with time?

# Solution

Received a pcap file, use Wireshark. Hint was "What protocol is associated with time?". 
It was NTP. Filter out "NTP", there are a few to look through, noticed we can construct the flag frame by frame. 
Simpler way right click any of the frames, Follow > UDP Stream. We can see it easier and construct the full flag.

````
#.
.............eS.b....eS.y....eS.u....eS.c....#.
.............eS.t....eS.f....eS.{....eS.S....#.
.............eS.0....eS._....eS.M....eS.y....#.
.............eS._....eS.P....eS.4....eS.r....#.
.............eS.4....eS.d....eS.0....eS.x....#.
.............eS._....eS.!....eS.d....eS.3....#.
.............eS.4....eS._....eS.D....eS.!....#.
.............eS.d....eS.n....eS.t....eS._....#.
.............eS.W....eS.0....eS.r....eS.k....#.
.............eS.}....eS......eS......eS......
````

# Flag

byuctf{S0_My_P4r4d0x_!d34_D!dnt_W0rk}

# Solved By

SniperKill258
