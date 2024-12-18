# C3
**Flag:** `picoCTF{adlibs}`

Only has a 31% upvote ratio on picoCTF for a reason - feels extremely unfun.
Code speaks louder than words:

```py
lookup1 = "\n \"#()*+/1:=[]abcdefghijklmnopqrstuvwxyz"
lookup2 = "ABCDEFGHIJKLMNOPQRSTabcdefghijklmnopqrst"

out = ""
ciper = "DLSeGAGDgBNJDQJDCFSFnRBIDjgHoDFCFtHDgJpiHtGDmMAQFnRBJKkBAsTMrsPSDDnEFCFtIbEDtDCIbFCFtHTJDKerFldbFObFCFtLBFkBAAAPFnRBJGEkerFlcPgKkImHnIlATJDKbTbFOkdNnsgbnJRMFnRBNAFkBAAAbrcbTKAkOgFpOgFpOpkBAAAAAAAiClFGIPFnRBaKliCgClFGtIBAAAAAAAOgGEkImHnIl"

prev = 0
for char in ciper:
  v = lookup2.index(char)
  # v = (cur - prev) % 40
  for i in range(100000):
    if (i - prev) %40 == v:
      out+=lookup1[i]
      prev=i
      break

print(out)

# P2
for i in range(1,len(out)):
	try:
		print(out[i**3], end='')
	except:
		break
```

# Custom encryption
**Flag:** `picoCTF{custom_d2cr0pt6d_49fbee5b}`

Lovely one, just reverse the layers one by one - just make sure that
you notice the xor is not a traditonal xor, but also reverses the string

```py
def generator(g, x, p):
    return pow(g, x) % p

def decrypt(cipher, key):
    plaintext = []
    for char in cipher:
        plaintext.append(chr(int(char / (key*311))))
    return plaintext

def dynamic_xor_decrypt(ciphertext, text_key):
    plain_text = ""
    key_length = len(text_key)
    for i, char in enumerate(ciphertext):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char
    return plain_text[::-1]

cipher = [61578, 109472, 437888, 6842, 0, 20526, 129998, 526834, 478940, 287364, 0, 567886, 143682, 34210, 465256, 0, 150524, 588412, 6842, 424204, 164208, 184734, 41052, 41052, 116314, 41052, 177892, 348942, 218944, 335258, 177892, 47894, 82104, 116314]

def tset(cipher, text_key):
	a = 90; b = 26
	p = 97; g = 31
	v = generator(g, b, p)
	key = generator(v, a, p) # assume key == b_key
	semi_cipher = decrypt(cipher, key)
	plain_text = dynamic_xor_decrypt(semi_cipher, text_key)
	print(f'plain text is: {plain_text}')

tset(cipher, "trudeau")
```

# miniRSA
**Flag:** `picoCTF{n33d_a_lArg3r_e_d0cd6eae}`

Firstly a [factordb check](https://factordb.com/index.php?id=1100000001346282488). Turns up nothing. Oh well -

Code first:
```py
>>> from sympy import integer_nthroot
>>> root, is_exact = integer_nthroot(k, 3)
>>> is_exact
True
>>> print(root)
13016382529449106065894479374027604750406953699090365388203722801043052339225981
# uhhhhhhh how do I make this a string?
# looks up wirteups
# :P
>>> bytearray.fromhex(format(root, 'x')).decode()
'picoCTF{n33d_a_lArg3r_e_d0cd6eae}'
```

So some googling around gives you this [link](https://crypto.stackexchange.com/questions/18301/textbook-rsa-with-exponent-e-3)
which seems too easy to be true but seemed worth a shot - and it was. Wish I came up with how to make the
string though.

Might impliment rsa from scratch to actually understand it - dosent look that hard really.

(on that note, starting to love sympy - works beautifully well and is extremely nice to work with).
