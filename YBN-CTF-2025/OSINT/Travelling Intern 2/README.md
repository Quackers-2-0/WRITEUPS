# Challenge Name

Travelling Intern 2

# Author

Firefly

# Description

Refer to the assignment brief for details and objectives Flag format is YBN{location_name_all_lowercase_no_spaces}

E.g. if the location is Resorts World Sentosa, the flag will be YBN{resorts_world_sentosa}

# Assignment Brief

Good job on finding William Kronx’s hotel, we have deployed agents to gather field intelligence on the target and his companions. Unfortunately, it seems like William has caught wind of ICA movement and has started to obfuscate his movements and meetups to prevent agents from keeping up with him. 

We noticed that William has made a comment on one of the intern’s social media posts but deleted it before we were able to read it. Recover the original comment that William has made, maybe it contains some hints on the next meetup with his… business associates.

# Solution

An archived version of the Instagram post, https://www.instagram.com/p/DSEJlHOEzTe/, was retrieved using archive.ph, https://archive.ph/. A Base64-encoded string was extracted, MzEuMTQzMzUwNCwxMjEuNjU3OTA3NQ==, and decoded to geographic coordinates, 31.1433504,121.6579075. Mapping the coordinates using Google Maps revealed Shanghai Disneyland Park.

# Flag

YBN{shanghai_disneyland_park}

# Solved By

SniperKill258
