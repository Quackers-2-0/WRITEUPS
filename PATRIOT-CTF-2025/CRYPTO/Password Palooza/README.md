# Password Palooza

**Event:** PatriotCTF 2025  
**Category:** Crypto

## What I did

The challenge gave me an MD5 password hash and a hint that the password ended with two numbers. Rather than brute-forcing every possible password, I used the `rockyou.txt` wordlist and had Hashcat try two digits after each word.

The attack mode was **hybrid wordlist + mask**, which is `-a 6` in Hashcat. The MD5 mode is `-m 0`. The command below shows the setup, with the hash saved in `hash.txt`:

```bash
hashcat -m 0 -a 6 hash.txt /usr/share/wordlists/rockyou.txt '?d?d'
```

That recovered the password `mr.krabbs57`. This was a nice reminder that a small clue about a password's format can make a dictionary attack much faster.

## Flag

```text
pctf{mr.krabbs57}
```

## Solved By

Kazikiri
