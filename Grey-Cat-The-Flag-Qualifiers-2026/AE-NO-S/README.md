# Challenge Name

AE-NO-S

# Author

N00bcak

# Description

So you know how the S in AES does not
stand for SubBytes, but rather Standard?

That clearly means SubBytes is NOT
NECESSARY !!!! :D ... right?

# Solution

AES SubBytes step has been removed:

````
for round_index in range(1, N_ROUNDS):
    _shift_rows(state)
    _mix_columns(state)
    _add_material(state, schedule[round_index])
````

To decrypt any ciphertext:

- XOR the ciphertext with the zero ciphertext.
- Multiply the result by the inverse matrix.
- Convert the recovered bits back into bytes

Every 16-byte reveals the original plaintext.

````
import json

# ---------- GF(2) matrix helpers ----------

def bytes_to_bits(b):
    bits = []
    for x in b:
        for i in range(7, -1, -1):
            bits.append((x >> i) & 1)
    return bits

def bits_to_bytes(bits):
    out = bytearray()
    for i in range(0, len(bits), 8):
        v = 0
        for b in bits[i:i+8]:
            v = (v << 1) | b
        out.append(v)
    return bytes(out)

def invert_matrix_gf2(rows):
    n = len(rows)

    # augment with identity
    aug = []
    for i in range(n):
        aug.append(rows[i] | (1 << (n + i)))

    # gauss-jordan
    for col in range(n):
        pivot = None
        for r in range(col, n):
            if (aug[r] >> col) & 1:
                pivot = r
                break

        if pivot is None:
            raise ValueError("Matrix not invertible")

        aug[col], aug[pivot] = aug[pivot], aug[col]

        for r in range(n):
            if r != col and ((aug[r] >> col) & 1):
                aug[r] ^= aug[col]

    mask = (1 << n) - 1
    inv = [(row >> n) & mask for row in aug]
    return inv

def apply_matrix(inv_rows, vec_bits):
    result = [0] * len(inv_rows)

    for i, row in enumerate(inv_rows):
        parity = 0
        for j, bit in enumerate(vec_bits):
            if bit and ((row >> j) & 1):
                parity ^= 1
        result[i] = parity

    return result

# ---------- load challenge data ----------

with open(r"FILE_DIRECTORY/output.txt") as f:
    data = json.load(f)

zero_ct = bytes.fromhex(data["zero"]["ct"])

# Build matrix columns
columns = []

for pair in data["basis_pairs"]:
    ct = bytes.fromhex(pair["ct"])

    diff = bytes(a ^ b for a, b in zip(ct, zero_ct))
    columns.append(bytes_to_bits(diff))

# Convert columns -> row representation
n = 128
rows = [0] * n

for col in range(n):
    for row in range(n):
        if columns[col][row]:
            rows[row] |= (1 << col)

print("[+] Inverting 128x128 matrix...")
inv_rows = invert_matrix_gf2(rows)

flag_ct = bytes.fromhex(data["flag_ct"])

plaintext = b""

for blk in range(0, len(flag_ct), 16):
    c = flag_ct[blk:blk+16]

    y = bytes(a ^ b for a, b in zip(c, zero_ct))
    y_bits = bytes_to_bits(y)

    p_bits = apply_matrix(inv_rows, y_bits)
    p = bits_to_bytes(p_bits)

    plaintext += p

print("\nRecovered plaintext:")
print(plaintext)
````

# Flag

grey{iT5_4LL_l1N3R_aLGyBeR?_a1WaY5_HaZ_B1n...}

# Solved By

SniperKill258
