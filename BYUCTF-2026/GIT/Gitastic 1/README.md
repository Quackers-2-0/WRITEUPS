# Challenge Name

Gitastic 1

# Description

The challenge said an employee had been leaking company information through the messages attached to code changes. We had to find the secret in a Git repository.

# Solution

This one was more about knowing where Git keeps information than about exploitation.

I cloned the challenge repository onto Kali and started by looking at its history:

```bash
git log
git log -p
```

There were a lot of commits, and I noticed long hexadecimal-looking strings in the history. At first I thought the flag might be encoded in those strings.

I copied the output to a text file and used ChatGPT to help search it. The challenge description was the key: it specifically mentioned **messages attached to code changes**, which points to **commit messages**, not necessarily the changes inside the files.

In the saved log, commit `04bbf589b31fd200370c974656c31bf3e205223e` had the flag inside its commit message. There was also another `byuctf{...}` string in an **author field**, but that belonged to **Gitastic 2**, not this challenge.

I confirmed the first flag was correct and then continued working on the later Gitastic challenges separately.

A quick way to check this kind of repository is:

```bash
git log --all --format='%H %B' | grep -i 'byuctf'
```

That command is a useful shortcut for future challenges; the commands I actually recorded using during this solve were `git log` and `git log -p`.

The lesson was to pay attention to the wording. Git stores plenty of things besides the current files, including commit messages, author names, tags and older versions of files.

# Flag

```text
byuctf{I_hop3_y0u_didnt_s3@rch_manually}
```

# Solved By

Kazikiri
