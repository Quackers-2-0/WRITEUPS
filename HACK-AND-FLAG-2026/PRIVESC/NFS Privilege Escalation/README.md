# Challenge Name

NFS Privilege Escalation

# Description

The target exposed a user's home directory over NFS. I needed to use the share to get access and eventually retrieve the root flag.

# Solution

I first checked the network services with `rpcinfo` and used `showmount` to see what NFS was sharing:

```bash
showmount -e 10.10.255.109
```

That showed `/home/john` exported to the CTF networks. I mounted it on Kali and tried creating a file, but my normal Kali account had UID 1000 while John's directory belonged to UID 1001. The server wasn't letting that user write there.

Using the share with a matching UID let me place an SSH public key and log in as `john`. I spent some time searching for flags and checking sudo, SUID programs and scheduled tasks, but nothing obvious worked.

The important clue was in the NFS export settings:

```text
/home/john ... (rw, ... , no_root_squash, ...)
```

`no_root_squash` means the NFS server **trusts root on the client as root on the server**. I checked that by creating a test file from Kali with `sudo` and confirming it appeared on the mounted share with UID 0.

That meant I could use the share to place a root-owned executable with the SUID bit, then run it on the target. The working approach used a copy of Bash on the share and set its owner and permissions from Kali. Running that copy on the target with `-p` gave a privileged shell.

After getting the root flag, I removed the files and SSH key I had added and unmounted the share.

This was a great example of why NFS settings matter. I didn't need a kernel exploit. The server was already trusting the wrong machine with root permissions.

# Flag

```text
HNF26{r00t_squ4sh_w4s_0ff}
```

# Solved By

Kazikiri
