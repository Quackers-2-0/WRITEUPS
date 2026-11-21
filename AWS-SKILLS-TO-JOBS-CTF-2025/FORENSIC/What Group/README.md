# What Group

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Forensic

## What I did

This challenge continued from an earlier stage where I downloaded a ZIP of Windows logs. I extracted the logs and looked through the **Security** events to find a newly created local group.

The useful event was **4731**, which records the creation of a security-enabled local group. Filtering the log and checking the newest matching event gave me the group name `thesmiths`.

It was good practice for finding a specific action in a large Windows event log instead of reading every entry one by one.

## Answer

```text
thesmiths
```

## Solved By

Kazikiri
