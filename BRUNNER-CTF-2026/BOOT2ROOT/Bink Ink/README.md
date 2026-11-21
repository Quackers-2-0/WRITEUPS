# Challenge Name

Bink Ink (User and Root)

# Description

Bink Ink was a Boot2Root challenge built around a firmware-update service. I had to get through a Squid proxy, reach the internal update tool, then use mistakes in its file handling to get the user and root flags.

# Solution

The public service was behind **Squid**, and the real update program was a Python command-line tool running at `127.0.0.1:30005`. The challenge source made one thing clear: the tool wasn't an HTTP server. It just read one line at a time using `input()` and understood commands like `get`, `list`, `verify` and `exit`.

That gave me an idea. What if the proxy forwarded an HTTP request to the internal port and the tool treated parts of that request as its own commands?

I first tried more normal proxy methods, but `CONNECT` was blocked and ordinary GET requests didn't give me the command format I needed. A raw **POST request** worked because Squid forwarded the body and the tool read the body lines as commands.

For example, the body could be:

```text
list\r\n
exit\r\n
```

The exact CRLF newlines and `Content-Length` mattered. Once I could send commands reliably, I tested the `get` function with a harmless download to make sure it could write a file in `/tmp/firmwares`.

## Getting the user flag

The `get` command took a URL with a `firmware` parameter and saved the downloaded content using something like:

```python
os.path.join('/tmp/firmwares', firmware_name)
```

This had a problem: if `firmware_name` was an **absolute path**, Python ignored the `/tmp/firmwares` base directory. So a name beginning with `/home/brunner_operator/` let me save a file there instead.

I used that to replace the user's firmware verifier:

```text
/home/brunner_operator/bink_ink_verify.sh
```

The replacement script only needed to print the user flag:

```bash
#!/bin/bash
cat /home/brunner_operator/flag_user.txt
exit 0
```

After uploading it, I called `verify` on an ordinary probe file under `/tmp/firmwares`. The path being verified was valid, but the program doing the verification was now **my script**. Its output contained the user flag.

## Getting the root flag

The root stage came from how the updater installed firmware. The `brunner_operator` account could run one command with sudo:

```text
sudo /bin/bink_ink_apply.sh <archive>
```

That root-owned installer called the same user-controlled verifier to check an update, then extracted a SquashFS firmware image directly onto `/`:

```bash
unsquashfs -f -d / firmware.bin
```

So I could make the verifier approve my update and use the update to **replace a file on the root filesystem**. The working approach built an archive containing a replacement `/bin/bink_ink_apply.sh` that would print `/root/flag_root.txt`.

There was one snag. The installer returned an error after it had already extracted the malicious image, so trying to install and run it in one script wasn't reliable. I split it into two stages:

- The first stage built and installed the SquashFS image, replacing the privileged script.
- The second stage replaced the verifier with a short script that ran `sudo /bin/bink_ink_apply.sh` again.

That second run executed the replaced program as root and returned the root flag.

The local challenge copy had `brunner{REDACTED}` placeholders in its flag files. The real flags below come from the live-result notes, not from those local placeholders. My saved notes also say I restored the changed verifier and installer and cleaned up the temporary files afterward.

This one was a chain of small mistakes: a proxy carrying commands to something that wasn't HTTP, an unsafe file path, and a root program trusting a script that a lower-privilege user could replace.

# Flags

**User:**

```text
brunner{Y0u_B1nk3d_th4t_1nk!!}
```

**Root:**

```text
brunner{d0nt_have_hi_priv_scr1pts_th4t_bl1ndly_tru5t_l0w_pr1v_scr1pt5}
```

# Solved By

Kazikiri
