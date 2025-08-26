---
layout: post
title: "Python For Offensive Security Encryption & Hashing"
date: 2025-08-03
categories: [encryption]
---

**- بسم الله**

---

## - *Encryption*

**Encryption:** *is a process of converting readable data `plaintext` to unreadable data `cipher text`.*

- **this process will done using `encryption` keys**

---

- ### How Encryption Works ?

- **Using mathematical operations between the `plaintext`  and `key`**
- **Then this will create the `cipher text` `Encrypted`**
---
- **There are two types of encryption :**

| Symmetric | Asymmetric |
|-----------|------------|
| Same Key Used for `Encryption` and `Decryption` | There is **Public Key** for `Encryption` and **Private Key** for `Decryption` |
| Faster Than Asymmetric but *Not Secure as Asymmetric* | Slower Than Symmetric but *More Secure* |
| **Example:** `AES`, `ChaCha20`, `RC4` | **Example:** `RSA` |

---

*There is 2 modules we will use in this part*:

**1. cryptography:** *`pip install cryptography`*

**2. pycryptodome**: *`pip install pycryptodome`*

---

**Lets Start :**

### AES (`Advanced Encryption Standard`)

**its the most popular `symmetric` encryption algorithm**
- *AES can be implemented using different modes, every mode has his way to encrypt the data, the most popular of them:*

1. **ECB:** Simplest Mode `Not that Secure`
2. **CBC**: **Most used** and safer than **ECB**
3. **GCM**: Perfect and modern used in `HTTPS`


- **AES** used in :

1. **HTTPS/TLS**
2. **WPA2/WPA3**
3. **Operating Systems.....**
4. **And Maybe in `VPN`'s**

---

- Now lets go to the practical part:

using [PyCryptoDome](https://pypi.org/project/pycryptodome/)

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import pad

key = b"notsecret1234566"

aes = AES.new(key=key,mode=AES.MODE_CBC,iv=key)

enc = aes.encrypt(pad(b"Hello World",block_size=16))
enc2 = aes.encrypt(pad(b"Hello World",block_size=16))


print("Encrypted Data:")
print(enc)
print(enc2)
```
- Output :
```bash
b'3?\xde|X\x98\xb2\xc1\x19\x9f\x8f\xafR\xc5\xc8\x82' # enc
b'\x94\xb8*\x06\x1cy\xceLMuq\xfd\x18K\xc3\x92' # enc2
```

- Now Lets Decrypt :

```python
from Crypto.Cipher import AES
from Crypto.Util.Padding import unpad

key = b"notsecret1234566"

aes = AES.new(key=key,mode=AES.MODE_CBC,iv=key)

enc = b'3?\xde|X\x98\xb2\xc1\x19\x9f\x8f\xafR\xc5\xc8\x82'
enc2 =b'\x94\xb8*\x06\x1cy\xceLMuq\xfd\x18K\xc3\x92'

dec = unpad(aes.decrypt(enc),block_size=16)
dec2 = unpad(aes.decrypt(enc2),block_size=16)

print("Decrypted Data:")
print(dec)
print(dec2)
```

- Output:
```
Decrypted Data:
b'Hello World'
b'Hello World'
```

- Key must be (16,24,32,64) byte
- Use padding functions to enseure that data length aligned to 16
- Note that encrypted data are different this is beacuse of iv
- Thats why is `CBC` is kinda safe


### RSA (`Rivest–Shamir–Adleman`)

**RSA is the most popular `asymmetric` encryption algorithm**  
- *It uses a `pair of keys`: a `public key` to encrypt data and a `private key` to decrypt it.*  
- *RSA is widely used for secure communication, digital signatures, and key exchange.*

- **RSA key concepts:**
1. **Public Key:** Can be shared with anyone to encrypt messages.
2. **Private Key:** Must be kept secret; used to decrypt messages.
3. **Key Size:** Usually 2048-bit or 4096-bit for modern security.

- **RSA is used in:**
1. **HTTPS/TLS** (for exchanging AES session keys)
2. **Email Encryption** (PGP/GPG)
3. **Digital Signatures** (authentication)
4. **VPNs** (for key exchange)

---

- Now let's go to the practical part using [PyCryptoDome](https://pypi.org/project/pycryptodome/):

- Generating Keys

```python
from Crypto.PublicKey import RSA
from Crypto.Cipher import PKCS1_OAEP

# Generate RSA keys (public & private)
key = RSA.generate(2048)
private_key = key.export_key()
public_key = key.publickey().export_key()

print("Private Key:", private_key[:50], "...")  # shortened for display
print("Public Key:", public_key[:50], "...")
```
- Decrypt with private key

```python
# Load private key
priv_key = RSA.import_key(private_key)
cipher_rsa = PKCS1_OAEP.new(priv_key)

dec = cipher_rsa.decrypt(enc)
print("Decrypted Data:", dec)

```

---

### # XOR Encryption

**! its not an algorithm but its way of encryption depends on `XOR` Logical Operation in bits level**

- Lets see how it works

lets suppose we want to encrypt `A` and our key is `B`

- Steps :
1. getting ascii values for **A** and **B**

A = 65, B = 66

2. convert it to binary

A = 01000001

B = 01000010

3. compare every bit if different result will be `1` otherwise `0`

C = 00000011

this is our encrypted character

now lets decrypt

**decryption rule :**

decrypted = encrypted `xor` key

C = 00000011

B = 01000010

decrypted = 01000001 -> `A`


- Lets see a python code :

```python
a = ord("A")
b = ord("B")
c = a ^ b
print(chr(c))

# decrypt

a = c ^ b

print(chr(a))

```

- Encryption Result = ♥

- this is for you, sorry the encrypted charachter :) -> not understandable

- Decryption Result = A

- Lets Make a XORER function

```python
def xorer(text,key):
    xored = ""
    for i in range(len(text)):
        xored += chr(ord(text[i]) ^ ord(key[i % len(key)]))

    return xored

key = "secret123"
enc = xorer("Hello World",key)
dec = xorer(enc,key)

print("Encrypted:",enc)
print("Decrypted:",dec)
```

```bash
Encrypted: ;☼▲
Tf]A▼☺
Decrypted: Hello World
```

---

## - Hashing

- ### What is hashing ?

- hashing is a process of converting big amount of data like **file** or **password** or **any data** to *fixed size value* we call it **hash**

**Example:** 5eb63bbbe01eeed093cb22bb8f5acdc3 

*This Hash For **hello world** in `md5` algorithm*

---

- ### How Hashing Works ?

- hashing can implemented using hashing algorithms like `md5`,`sha256`,`sha512` and more...

---

- ### Why use hashing ?

1. To ensure data integrity

2. another common use is to verify passwords. Instead of storing passwords in plain text in databases, we store the password hash. When we verify it, we convert the entered password to a hash and verify it.

- you will face this use a lot 

---

### - Practical Part

- we will use `hashlib` and `zyntra` modules

```python
import hashlib

password = "mypassword123"

hashed_md5 = hashlib.md5(password.encode()).hexdigest()
hashed_sha256 = hashlib.sha256(password.encode()).hexdigest()

print("MD5:",hashed_md5)
print("SHA256:",hashed_sha256)
```

```python
MD5: 9c87baa223f464954940f859bcf2e233
SHA256: 6e659deaa85842cdabb5c6305fcc40033ba43772ec00d45c2a3c921741a5e377
```

- hexdigest() to print hash in hex

- as you can see md5 is a weak hashing algorithm, we can break it eaiser than sha256,sha512

---

### - Breaking Hashes

- Now lets try making a md5 hash breaker

- **idea**:
    1. we will make a script that take arguments `hash` and `wordlist`
    2. in the script we will loop over the wordlist and compare the **hash**

### - MD5 Breaker

```python
from zyntra import FileObj
import hashlib
import sys

if len(sys.argv) < 3:
    print(f"Usage: python3 {sys.argv[0]} <hash> <wordlist>")
    exit(1)

hashed = sys.argv[1]
wordlist = sys.argv[2]


def check_hash(password: str):
    # getting hash for the password
    pass_hash = hashlib.md5(password.encode()).hexdigest()
    # compare the hash
    if pass_hash == hashed:
        print("Password Found:",password)
        exit(0)
    else:
        print("Trying",password)

f = FileObj(wordlist)
if not f.exist():
    print("[!] Wordlist Not Exist")
    exit(1)

passwords = f.stripped_lines() # read lines stripped

for password in passwords:
    check_hash(password)
```

- Lets Try Breaking this hash `482c811da5d5b4bc6d497ffa98491e38`

```python
python hashing.py 482c811da5d5b4bc6d497ffa98491e38 passwords.txt
```
```
Trying secret123
Trying mypassword
Trying helloworld
Trying 12345678
Trying 0987654321
Trying llllllllll
Password Found: password123
```

- Here we go

### - Challenge

- i want you to create sha256 hash breaker and optimize it using threading module


### - Ending

- i wish you love this part
- try to train yourself on many projects
- **مع السلامه**
