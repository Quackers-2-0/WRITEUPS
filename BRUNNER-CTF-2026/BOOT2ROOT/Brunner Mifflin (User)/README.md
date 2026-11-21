# Challenge Name

Brunner Mifflin (User)

# Author

Emil8250

# Description

The first part of Brunner Mifflin involved an HR system. The challenge wanted us to find the IT user's details and get the user flag.

# Solution

I started by looking at the supplied C# file to work out how the HR system's API was set up. It exposed a normal user endpoint, but the interesting route was:

```text
GET /api/User/Admin/{role}
```

The route name sounded like something that should be protected, but it didn't require the level of access its name suggested. After looking at how the route used the `role` parameter, I tried the IT role:

```bash
curl -i http://TARGET/api/User/Admin/itguy
```

The endpoint gave me information for the `itguy` account, including the first flag. The same information later helped me get a normal user shell for the root part.

This was a good reminder that a route having `Admin` in its name doesn't mean the server actually checks who is allowed to use it.

# Flag

```text
brunner{1tGuyW111F1x}
```

# Solved By

Kazikiri
