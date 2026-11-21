# Challenge Name

Secret Event

# Description

The challenge was a web app with an admin dashboard protected by a JSON Web Token (JWT). The challenge hinted that you couldn't just make things up.

# Solution

I noticed the site used a JWT and started looking for a way into the admin dashboard. I also downloaded some dashboard-related files, which gave me a lead on the signing key.

The token was using **HS256**. With help checking the token signature, we found that its signing secret was just:

```text
secret
```

That is an awful choice for an authentication secret because anyone who guesses or learns it can create a token that the server accepts.

The normal token had a payload along these lines:

```json
{"sub":"quack","name":"quack","role":"user"}
```

Changing `role` to `admin` and signing the new token with the same secret made it possible to access the admin page. After replacing the existing JWT cookie, I got the flag.

The problem wasn't JWT itself. It was trusting a token signed with such an easy-to-guess key.

# Flag

```text
brunner{well_known_secret}
```

# Solved By

Kazikiri
