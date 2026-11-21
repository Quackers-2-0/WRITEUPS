# What Tasks

**Event:** AWS Skills to Jobs CTF 2025  
**Category:** Forensic

## What I did

I kept working with the Windows event logs, this time looking for **scheduled tasks that had been deleted**.

I initially missed the entries because the relevant Task Scheduler logs were hidden. In Event Viewer, I had to open **View** and enable **Show Analytic and Debug Logs** to see the extra channels.

The event I needed was **141**, which records a task deletion in the Task Scheduler Operational log. Following those records let me recover three task names:

```text
DeliverUpdates
RevertUpdates
RunUpdates
```

The lesson was that a missing log entry doesn't always mean nothing happened; sometimes you're just looking in the wrong channel.

## Answer

```text
DeliverUpdates,RevertUpdates,RunUpdates
```

## Solved By

Kazikiri
