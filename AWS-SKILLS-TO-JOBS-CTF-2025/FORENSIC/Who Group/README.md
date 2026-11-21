# Who Group

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Forensic

## What I did

This continued from **What Group**. After finding the group named `thesmiths`, I needed to work out which users had been added to it.

In the Windows Security log, I looked for the group-creation and member-added events. The membership events showed **security identifiers (SIDs)** rather than the friendly usernames I needed.

I copied the five SIDs and searched the event log for each one. The later matching events let me connect them back to usernames. After that I arranged the names in alphabetical order.

The important part was following those SIDs across the log entries instead of expecting every event to show a readable name.

## Answer

```text
colin,ed,jonny,philip,thom
```

## Solved By

Kazikiri
