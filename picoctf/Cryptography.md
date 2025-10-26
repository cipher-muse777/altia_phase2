# 2. CUSTOM ENCRYPTION 
Can you get sense of this code file and write the function that will decode the given encrypted file content.
Find the encrypted file here flag_info and code file might be good to analyze and get the flag.

## SOLTUION 
- the given file revealed that it combined  XOR encryption with a multiplicative cipher with a shared key using the Diffie-Hellman formula
- the XOR key takes the message and mixes it up with the word "trudeau" and reversing the plaintext
- the multiplicative cipher, multiplies each letter by a particular number which was formed using Diffie-Hellman formula 

- so to solve it we must first reverse the multiplication (divide by shared_key × 311) and then reverse the XOR with "trudeau" again
```
 cipher.append(((ord(char) * key*311)))
```
refers to encrypted_number = ASCII_value × key × 311 so to decrypt we must ASCII_value = encrypted_number ÷ (key × 311)

- to solve the xor part we must first reverse the plaintext then we can just use the xor operation again which will simply give the original data back

- FINAL CODE
  
  ```
   def decrypt(cipher, key):
    plaintext = ""
    for num in cipher:
        # Reverse the multiplicative encryption: cipher = (char * key * 311)
        if num == 0:
            plaintext += chr(0)
        else:
            char_val = num // (key * 311)
            plaintext += chr(char_val)
    return plaintext

  def dynamic_xor_decrypt(cipher_text, text_key):
    plain_text = ""
    key_length = len(text_key)
    for i, char in enumerate(cipher_text):
        key_char = text_key[i % key_length]
        decrypted_char = chr(ord(char) ^ ord(key_char))
        plain_text += decrypted_char
    return plain_text[::-1]  # Reverse at the end since encryption was done in reverse

  def generator(g, x, p):
    return pow(g, x) % p

  def decode_flag(cipher):
    p = 97
    g = 31
    a = 95
    b = 21
    
    # Regenerate the shared key
    u = generator(g, a, p)
    v = generator(g, b, p)
    key = generator(v, a, p)
    
    # First decrypt the multiplicative cipher
    semi_cipher = decrypt(cipher, key)
    
    # Then decrypt the XOR cipher
    plaintext = dynamic_xor_decrypt(semi_cipher, "trudeau")
    
    return plaintext

  The cipher from the file
  cipher = [237915, 1850450, 1850450, 158610, 2458455, 2273410, 1744710, 1744710, 1797580, 1110270, 0, 2194105, 555135, 132175, 1797580, 0, 581570, 2273410, 26435, 1638970, 634440, 713745, 158610, 158610, 449395, 158610, 687310, 1348185, 845920, 1295315, 687310, 185045, 317220, 449395]

  Decode the flag
  flag = decode_flag(cipher)
  print(f"Decoded flag: {flag}")
```
````
## FLAG
picoCTF{custom_d2cr0pt6d_66778b34}

## CONCEPTS LEARNED 
- how XOR encryption takes the message and mixes it up using a particular word 

```
semi_cipher = dynamic_xor_encrypt(plain_text, text_key)
```
```
def dynamic_xor_encrypt(plaintext, text_key):
    cipher_text = ""
    key_length = len(text_key)
    for i, char in enumerate(plaintext[::-1]):           **Reverse the string**
        key_char = text_key[i % key_length]              **Get next key letter "t","r","u","d","e","a","u"**
        encrypted_char = chr(ord(char) ^ ord(key_char))  **XOR OPERATION**
        cipher_text += encrypted_char
    return cipher_text  **Returns XOR-scrambled text**
```

- how we can form shared key using Diffie-Hellman formula
```
   a = randint(p-10, p)
    b = randint(g-10, g)
    print(f"a = {a}")
    print(f"b = {b}")
```
```  
    # ========== SHARED KEY CALCULATION ==========
    u = generator(g, a, p)        # ← Part of shared key math
    v = generator(g, b, p)        # ← Part of shared key math  
    key = generator(v, a, p)      # ← FINAL SHARED KEY calculated
    b_key = generator(u, b, p)    # ← Same shared key from other side
    
    shared_key = None
    if key == b_key:
        shared_key = key          # ← This is the SECRET NUMBER (61 in our case)
    else:
        print("Invalid key")
        return
```

- how multiplicative cipher uses this shared key to decode it further
```
def encrypt(plaintext, key):
    cipher = []
    for char in plaintext:
        cipher.append(((ord(char) * key * 311)))  # ← MULTIPLICATION OPERATION
        # key is the shared_key (61)
        # Each letter gets multiplied by 61 × 311
    return cipher  # ← Returns the final encrypted numbers
```

## RESOURCES 
https://www.geeksforgeeks.org/computer-networks/implementation-diffie-hellman-algorithm/
https://www.geeksforgeeks.org/dsa/xor-cipher/
https://www.geeksforgeeks.org/computer-networks/what-is-multiplicative-cipher-in-cryptography/
https://www.pycryptodome.org/



