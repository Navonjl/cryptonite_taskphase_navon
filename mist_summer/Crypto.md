# EVEN RSA CAN BE BROKEN???

Flag: picoCTF{tw0_1$_pr!m381772be5}

We download the code and see nothing standing out - except for a setup module that is used to generate the RSA keys. Welp - we cant run it, so we go reading the keys.

```
❯ nc verbal-sleep.picoctf.net 51434
N: 17051063882989555422101872512011153955340868086536677589559206914956662880673077127636110741069024421858020287039845640149252391130124113953909722544611114
e: 65537
cyphertext: 13935011761174216927963529017338976951832956811117614127207680278816441155104429946879900116233503529649714069671167374964652818920732015876847538430514469
```

If we stare into the abyss long enough, it stares back at us. In this case, the abyss is a broken RSA implementation. 

if we look at N long enough, we can see that it is in fact even, which means that one of the prime factors is 2.

Well, anyways, here is a full solver:

```python
import socket
from Crypto.Util.number import long_to_bytes, inverse

with socket.create_connection(("verbal-sleep.picoctf.net", 51434)) as s:
    data = s.recv(4096).decode()

# N, e, cipher
out = [0, 0, 0]
out = list(map(int, data.split()[1::2]))

N = out[0]
e = out[1]
c = out[2]
# Step 1: Factor N (since it's even, we know one factor is 2)
p = 2
q = N // 2

# Step 2: Compute phi(N)
phi = (p - 1) * (q - 1)

# Step 3: Compute private key d
d = inverse(e, phi)

# Step 4: Decrypt ciphertext
decrypted_long = pow(c, d, N)
decrypted_bytes = long_to_bytes(decrypted_long)
decrypted_text = decrypted_bytes.decode("utf-8")

print(decrypted_text)
```

# transposition-trial

Flag: `picoCTF{7R4N5P051N6_15_3XP3N51V3_56E6924A}`

Look on my One-Liner, ye Mighty, and despair!
```python
ciper = "heTfl g as iicpCTo{7F4NRP051N5_16_35P3X51N3_V6E5926A}4"

print(*map(lambda x: x[-1] + x[:-1],
        (ciper[i*3:(i+1)*3] for i in range(len(ciper)//3)))
    ,sep='')
```
