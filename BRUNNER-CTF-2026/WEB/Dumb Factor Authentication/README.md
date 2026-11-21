# Challenge Name

Dumb Factor Authentication

# Description

The site used six-digit one-time PINs for login, had a warning about username collisions, and stored private feedback tickets. The flag was hidden in a private ticket.

# Solution

The login only asked for a six-digit PIN, with no username. That made me wonder if I could find **any valid PIN** rather than knowing one for a specific account.

I first looked at automating requests with Python, but ran into trouble with how the service responded. I switched to `ffuf` and tried PINs against the login endpoint. There were a lot of strange responses when I pushed the thread count too high, so I paid more attention to the **302 redirects**, which looked like successful logins.

Once I got into an ordinary account, the site warned about duplicate usernames. I tried changing my account's username to `admin` through the settings request, and it accepted the change:

```http
POST /settings
Content-Type: application/x-www-form-urlencoded

action=update_username&username=admin
```

I can't say for sure whether that username change was needed to read the flag. What definitely helped was the feedback system.

I submitted my own feedback and noticed its URL ended with:

```text
/feedback/view?id=25
```

That made me try the previous number:

```text
/feedback/view?id=24
```

Ticket 24 contained the flag.

The useful takeaway is to **check object IDs whenever a site creates something for you**. If you can change the ID and read someone else's private entry, the access checks aren't doing their job.

# Flag

```text
brunner{ch1ef_duck_0ff1c3r_4ppr0v3d_th1s_fl4g}
```

# Solved By

Kazikiri
