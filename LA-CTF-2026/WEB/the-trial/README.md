# Challenge Name

the-trial

# Author

apletl23

# Description

I think the main takeaway from Kafka is that bureaucracy is bad? Or maybe it's that we live in a society.

the-trial.chall.lac.tf

# Solution

The page contains a slider that generates a 4‑letter word using a custom base‑26 alphabet:
cm = "kjzhcyprdolnbgusfiawtqmxev"
The encoding is little‑endian base‑26. Setting the word to flag gives the slider value: 240932.
````<input type="range" min="0" max="456975" value="420417" id="val">````
Change the value to 240932 and submit, it will show the flag
````<input type="range" min="0" max="456975" value="240932" id="val">````

# Flag

lactf{gregor_samsa_awoke_from_wait_thats_the_wrong_book}

# Solved By

SniperKill258
