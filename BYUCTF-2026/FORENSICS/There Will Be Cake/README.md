# Challenge Name

There Will Be Cake

# Author

JC6143

# Description

The Enrichment Center is required to remind you that all
test subject activity will be logged, analyzed, and stored
for scientific purposes.

"Cake and grief counseling will be available at the
conclusion of the test."

*The pcap file contains a flag for each of the following challenges: "There will be cake", "Are you still there?", "Alright. Paradox time", and "Corrupted Cores".

If the flag you found doesn't work, then it most likely belongs to one of the other 3 challenges.

Hint: what is a baked treat similar to a cake that you can
find on almost any website?


# Solution

Received a pcap file, use Wireshark. Hint was "what is a baked treat similar to a cake that you can find on almost any website?" It was a cookie. 
Filter out "http", there are 2 logs look through and gotten the first frame.

````
Frame 15: Packet, 264 bytes on wire (2112 bits), 264 bytes captured (2112 bits) on interface \Device\NPF_{CDA6A612-BB85-4C72-BB0A-F44DBB5E070D}, id 0
Ethernet II, Src: VMware_c0:00:08 (00:50:56:c0:00:08), Dst: VMware_b7:fb:48 (00:0c:29:b7:fb:48)
Internet Protocol Version 4, Src: 192.168.132.1, Dst: 192.168.132.133
Transmission Control Protocol, Src Port: 54004, Dst Port: 8000, Seq: 1, Ack: 1, Len: 210
Hypertext Transfer Protocol
    POST / HTTP/1.1\r\n
    Host: 192.168.132.133\r\n
    Cookie: cake=Ynl1Y3Rme1RoM19DNGszXyFzXzRfTCEzX0hUQzU2emVFfQ==\r\n
        Cookie pair: cake=Ynl1Y3Rme1RoM19DNGszXyFzXzRfTCEzX0hUQzU2emVFfQ==
    X-Seq: 0\r\n
    Content-Type: application/json\r\n
    Content-Length: 24\r\n
    Connection: close\r\n
    \r\n
    [Response in frame: 18]
    [Full request URI: http://192.168.132.133/]
    File Data: 24 bytes
JavaScript Object Notation: application/json
    Object
````
Take the encrypted code, it was base64 "Ynl1Y3Rme1RoM19DNGszXyFzXzRfTCEzX0hUQzU2emVFfQ==" and use cyberchef or dcode to decrypt it.
Received this flag, "byuctf{Th3_C4k3_!s_4_L!3_HTC56zeE}"

# Flag

byuctf{Th3_C4k3_!s_4_L!3_HTC56zeE}

# Solved By

SniperKill258
