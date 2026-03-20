# Challenge Name

PowerSploit

# Author

Firefly

# Description

An alert has been raised from Northland SOC following a suspicious download of a PowerShell script onto one of the internal hosts on the network. 

Investigate this incident and identify any malicious activity.

Mirror 1: nc chall1.lagncra.sh 18339

Mirror 2: nc chall2.lagncra.sh 18339

# Solution

Example

Qn 1: What is the IP address of the C2 server?

Hint: ````*.*.*.*````

Ans: 172.16.100.12

Qn 2: What is the file name that system information is written to? Provide only the filename (e.g. index.html)   

Hint: ````*******.***````

Ans: ciminfo.txt

Qn 3: What registry key does the attacker attempt to write the persistence mechanism to?

Hint: ````****\********\*********\*******\**************\***````

Ans: HKCU\Software\Microsoft\Windows\CurrentVersion\Run

Qn 4: What is the name of the persistence mechanism given by the attacker?

Hint: Do not include the file extension (e.g. revshell.exe -> revshell)

Ans: ServiceUpdater

Qn 5: What port does the reverse shell connect to?

Hint: A port number (1-65535)

Ans: 443

Qn 6:  How long does the attacker connect to the victim workstation?

Hint: In seconds.

Ans: 110.588415

Qn 7: The attacker reads a confidential file on the victim workstation. What is the contents of the file?    

Hint: ````****************************************************************````

Ans: 0d550b3f9c28988953197836d9f2db696db24fbb64d9dd10e7c6e4ba5003e51f

Qn 8: The attacker attempted to create a new local administrator user. What is the name of the user account? 

Hint: ````********````

Ans: Ichinose

All questions answered correctly. Assignment complete.

Here is your flag: LNC26{15fa1a9dfb15498a80d08727cf2176c2} 

# Flag

LNC26{15fa1a9dfb15498a80d08727cf2176c2}

# Solved By

SniperKill258
