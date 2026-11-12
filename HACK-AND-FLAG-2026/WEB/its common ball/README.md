# Challenge Name

its common ball

# Description

This was a treasure hunt where the flag had been split into seven pieces across a website. Each clue pointed to a different place or HTTP method.

# Solution

The main page already had the opening `HNF26{`, so I knew I was collecting pieces of the same flag. I found `we13ome_2` in the cookie and another piece in `robots.txt`. The robots file also pointed me to `/theriddle`, where I found the next part.

The other clues weren't all about visiting pages normally. One told me to send a message, which led me to try a **POST request to the main page**. That gave me `c8t3g0ry_we`. Using **DELETE** on the main page returned the final piece.

The washing-machine rhythm-game clue was the annoying one. I first tried `/maimai`, but the site returned 404. I ended up checking `/maimai.html` manually, and that was the missing third part. I had overcomplicated it by trying a longer list of paths.

The pieces came from several different parts of the web application:

| Part | Where I found it | Piece |
| --- | --- | --- |
| 1 | Main page | `HNF26{` |
| 2 | Cookie | `we13ome_2` |
| 3 | `/maimai.html` | `_we3_` |
| 4 | `POST /` | `c8t3g0ry_we` |
| 5 | `robots.txt` | `_hope_y0u_` |
| 6 | `/theriddle` | `hav3_a` |
| 7 | `DELETE /` | `_1nteresting_t1m3}` |

Once I put them in order, the final flag was accepted.

This one was a good lesson in remembering that a web server can respond differently to **GET, POST, and DELETE**, even when you're using the same URL.

# Flag

```text
HNF26{we13ome_2_we3_c8t3g0ry_we_hope_y0u_hav3_a_1nteresting_t1m3}
```

# Solved By

Kazikiri
