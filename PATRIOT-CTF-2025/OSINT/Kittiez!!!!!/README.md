# Challenge Name

Kittiez!!!!!

# Author

vWing

# Description

Kittiez hehe

Note: Hints for this challenge cost points!

# Solution

OSINT hints led to the VX-Underground cat image archive. All image archives were downloaded and individually hashed using an MD5 hashing script. The image matching the target hash was identified, and its associated text revealed the final flag.

````
import os
import hashlib

folder = r"file_directory\Cats.00003"  # <-- path to your downloaded cat images
target_hash = "9c5ca692da8d6e489beecd5b448ddb35"

def md5_of_file(file_path):

     """Compute MD5 hash of a file in binary mode."""
     hash_md5 = hashlib.md5()
    try:
         with open(file_path, "rb") as f:
         for chunk in iter(lambda: f.read(4096), b""):
         hash_md5.update(chunk)
         return hash_md5.hexdigest()
    except Exception as e:
       print(f"Error reading {file_path}: {e}")
       return None

# Scan the folder
found = False
for filename in os.listdir(folder):
  filepath = os.path.join(folder, filename)
    if os.path.isfile(filepath):
    file_hash = md5_of_file(filepath)
    if file_hash == target_hash:
     print(f":white_check_mark: Found matching image: {filename}")
     found = True
         break
if not found:
   print(":x: No matching image found in this folder.")
````

# Flag

pctf{SHOUTOUT_TO_SILLY_CATS}

# Solved By

SniperKill258
