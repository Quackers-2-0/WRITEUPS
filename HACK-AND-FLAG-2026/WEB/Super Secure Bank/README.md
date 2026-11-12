# Challenge Name

Super Secure Bank

# Description

The bank login used JWTs. I had to get access beyond an ordinary account and find the admin flag.

# Solution

I started testing the bank's endpoints and found `/api/debug`. It was behaving strangely: a normal request complained about a missing environment body, while POST requests to it didn't work.

The important request ended up being **GET with a JSON body**:

```bash
curl -i -X GET 'http://10.10.10.10:8003/api/debug'   -H 'Content-Type: application/json'   -d '{"environment":"development"}'
```

The response leaked the JWT signing secret. That was the main weakness: a debug feature was handing out the key used to trust login tokens.

I used **jwt.io** to create a new HS256 token containing an admin role. My first test still returned `Unauthorized`, so getting a correctly signed token wasn't the whole story. Eventually I tried the flag endpoint itself:

```text
/api/flag
```

That returned the flag.

The lesson here was to look closely at debug endpoints and how an application checks JWT claims. Even a valid token may behave differently depending on the route you're accessing.

# Flag

```text
HNF26{n0_qual1ty_A33uranc3_l0l}
```

# Solved By

Kazikiri
