# 1. ALL SIGNS ALLIGN 

from random import randint
from Crypto.Util.number import *
from secret import flag

p = 9129026491768303016811207218323770273047638648509577266210613478726929333106121387323539916009107476349319902011390210650434835260358014251332047605739279

def get_x():
    while True:
        x = randint(2,p-1)
        if pow(x,(p-1)//2,p) == 1:
            break
    return x

def get_y():
    return -get_x()%p

a = randint(2,p-1)
out = []

for b in bin(bytes_to_long(flag))[2:]:
    if b == '0':
        out.append((a*get_x())%p)
    else:
        out.append((a*get_y())%p)

with open('out.txt','w') as f:
    f.write(str(out)+'\n')
## FLAG 
nite{r3s1du35_f4ll1ng_1nt0_pl4c3}

## SOLUTION 
- we are given a very large prime no p which ends with 79 and when we divide 79 by 4 we get remiander 3 hence  ```p = 3 (mod 4)```
- we have a get_x() function that finds a random number x between 2 and p-1 that is quadratic residue modulo p
- we also have a get_y(x) function that returns -x%p which creates our quadratic non residue number because for primes where p ≡ 3 (mod 4), -1 is always a QNR, so multiplying any QR by -1 yields a QNR 
- now since our flag is converted to binary and for each binary bit if b=0, output is ```(a * get_x()) % p``` where get_x() returns a random quadratic residue modulo p AND if b=1, Output ```(a * get_y()) % p``` where get_y() returns ```-get_x() % p```, guaranteed to be a quadratic non-residue since ```p = 3 (mod 4)```
- our a value is fixed but unknown so we have to try the two possible mappings between quadratic residuosity and bit values
- now for each value c from out.txt file if we can compute the legendre value and then try both mappings where qr(legendre==1) = 0 and qnr(legendre==p-1) = 1 AND where qr(legendre==1) = 1 and qnr(legendre==p-1) = 0
- converting the bit sequence to bytes will give us the flag

 ## FINAL CODE
 ```
def bytes_to_long(s):
    return int.from_bytes(s, 'big')

def long_to_bytes(n):
    return n.to_bytes((n.bit_length() + 7) // 8, 'big')

p = 9129026491768303016811207218323770273047638648509577266210613478726929333106121387323539916009107476349319902011390210650434835260358014251332047605739279

# Load output
with open('out.txt') as f:
    out = eval(f.readline().strip())

def legendre_symbol(c, p):
    ls = pow(c, (p-1)//2, p)
    return 1 if ls == 1 else -1

# Compute Legendre symbols for all outputs
legs = [legendre_symbol(c, p) for c in out]

def bits_to_bytes(bits):
    # Convert entire bit list to one big integer
    n = 0
    for bit in bits:
        n = (n << 1) | bit
    # Convert to bytes
    return long_to_bytes(n)

# Mapping 1: QR=0, QNR=1
bits1 = [0 if l == 1 else 1 for l in legs]
try1 = bits_to_bytes(bits1)

# Mapping 2: QR=1, QNR=0
bits2 = [1 if l == 1 else 0 for l in legs]
try2 = bits_to_bytes(bits2)

print("Mapping 1 (QR=0):", try1)
print("Mapping 2 (QR=1):", try2)

# Also try with the first few bits to see if we can spot the flag pattern
print("\nFirst 50 chars of each:")
print("Mapping 1:", try1[:50])
print("Mapping 2:", try2[:50])
```

## CONCEPTS LEARNED 
- quadratic residue (QR) is a number that has a square root in modular world {r^2 = x (mod p)}
- quadratic non residue (QNR) is a number that does not have a square root in modular world { no r such that r^2 = x (mod p)}
- this works because of Euler's Criterion for the Legendre symbol
- and we check this by ```legendre = pow(x, (p-1)//2, p) == 1``` if this is true then its a QR
- ```legendre = pow(x, (p-1)//2, p) == p-1``` if this is true then its a QNR
- multiplicative property of qr:
  QR * QR = QR
  QR * QNR = QNR
  QNR * QNR = QR

## RESOURCES
- https://en.wikipedia.org/wiki/Quadratic_residue
- https://brilliant.org/wiki/legendre-symbol/
- https://www.geeksforgeeks.org/dsa/modular-exponentiation-power-in-modular-arithmetic/
- https://www.pycryptodome.org/

# 3. QUIXORTE 
import os

def rotate(b,i):
    return ((b>>i%8)|(b<<(8-i%8))) & 0xFF

def encrypt(flag,key):
    enc = bytearray(rotate(x,i) for i,x in enumerate(flag))
  
  for i in range(len(enc) - len(key) + 1):
        for j in range(len(key)):
            enc[i+j] ^= key[j]
      return enc
    
key = os.urandom(8)

with open('quote.png','rb') as f:
    flag = f.read()

enc = encrypt(flag,key)

with open('quote.png.enc','wb') as f:
    f.write(enc)

## FLAG 
nite{to_b3_X0R_n0t_t0_b3333}

## SOLUTION 
- when we open the given png file in a hex editor we can see that the first 8 bytes is different from the usual 89 50 4E 47 0D 0A 1A 0A for png files

<img width="1910" height="926" alt="Screenshot 2025-11-30 125905" src="https://github.com/user-attachments/assets/ef466379-1df8-4e6a-afa1-e2569c779d68" />

- the given python program also explains how each byte is rotated by the number of bits equal its index%8 ie, 0 byte is 0,7 byte is 7, 8 byte is 0 and 9 byte is 1 this is what scrambles the png header
- there is also an 8-byte random key ```(from os.urandom(8))``` is used to XOR the data in a sliding window so each byte gets xored with key[0] till key[7] and these windows overlap therefore modifying each byte multiple times
- 

