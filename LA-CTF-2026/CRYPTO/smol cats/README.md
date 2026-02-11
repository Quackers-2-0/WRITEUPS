# Challenge Name

smol cats

# Author

burturt

# Description

My cat walked across my keyboard and made this RSA implementation, encrypting the location of the treats they stole from me! However, they already got fed twice today, and are already overweight and needs to lose some weight, so I cannot let them eat more treats.
Can you defeat my cat's encryption so I can find their secret stash of treats and keep my cat from overeating?

nc chall.lac.tf 31225

# Solution

Example

nc chall.lac.tf 31224, we are given (everyone is different everytime u connect)

````
n = 1065164940413730322028928826023076966895569390269418018971589
e = 65537
c = 1049422542467127029196202546726197988533321933537622185560856
````

Factor the modules, used https://www.alpertron.com.ar/ECM.HTM, paste n into it and factorise.

````
p = 1019322416371941041328541966213
q = 1044973526830652746381648057153
````
Make a whole python script to compute private key and decrypt cipher text (c)

````
n = 1065164940413730322028928826023076966895569390269418018971589
e = 65537
c = 1049422542467127029196202546726197988533321933537622185560856
p = 1019322416371941041328541966213
q = 1044973526830652746381648057153
phi = (p - 1) * (q - 1)
d = pow(e, -1, phi)
m = pow(c, d, n)
Output the number of treats
print(m)
````

Use the (m) and paste to the answer, [mrrrow? How many treats do I want?] 

# Flag

lactf{sm0l_pr1m3s_4r3_n0t_s3cur3}

# Solved By

SniperKill258
