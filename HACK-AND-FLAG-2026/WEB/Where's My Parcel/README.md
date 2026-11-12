# Challenge Name

Where's My Parcel

# Description

The parcel lookup page only answered whether a reference existed. An old report was locked behind a four-digit code, and the challenge hinted that a yes-or-no answer might still leak useful information.

# Solution

I initially thought about brute-forcing the four-digit unlock code, but the challenge text made me curious about the reference-checking endpoint instead.

The page looked like it only allowed four characters, but that restriction wasn't actually enforced by the server. I sent SQL-looking input directly with `curl` and got an interesting response:

```bash
curl -s -X POST http://10.10.10.10:8004/verify   --data-urlencode "reference=' OR '1'='1' -- "
```

That returned **Reference found**. Changing the condition to something false returned **No matching reference**:

```bash
curl -s -X POST http://10.10.10.10:8004/verify   --data-urlencode "reference=' OR '1'='2' -- "
```

That difference confirmed a **boolean-based blind SQL injection**. I didn't need the site to print database results; I only needed its yes-or-no answer.

I then used `sqlmap` against the `reference` POST parameter with SQLite and boolean-based testing. It found the `archive_access` table and its `report_id` and `access_code` columns:

```text
CREATE TABLE archive_access (
    id INTEGER PRIMARY KEY,
    report_id TEXT UNIQUE NOT NULL,
    access_code TEXT NOT NULL
)
```

The rest of the old report unlock process isn't fully in my saved chat, but I eventually recovered the code and got the flag.

The clever part of the challenge was that **a yes-or-no response can still leak data** when you can control the condition being checked.

# Flag

```text
HNF26{1_b1t_4t_4_t1m3}
```

# Solved By

Kazikiri
