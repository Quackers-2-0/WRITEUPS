# What Created

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Forensic

## What I did

This was another Windows Task Scheduler investigation. I needed to find a task that a user had registered.

I first went down the wrong path after seeing references to Windows Security event **4698**. But the event I needed wasn't in the logs I was checking.

Instead, I searched the Task Scheduler Operational events for **Task Registered**. Event **106** showed the registration, and it led me to the task named `ChangeBackground`.

It taught me to pay attention to **which event log** a guide is talking about. Two different Windows logs can describe similar actions using different event IDs.

## Answer

```text
ChangeBackground
```

## Solved By

Kazikiri
