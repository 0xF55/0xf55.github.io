## # *Encryption*

**Encryption:** *is a process of converting readable data `plaintext` to unreadable data `cipher text`.*

- **this process will done using `encryption` keys**
___

- ### How Encryption Works ?

- **Using mathematical operations between the `plaintext`  and `key`**
- **Then this will create the `cipher text`->`Encrypted`**
---
- **There are two types of encryption :**

|Symmetric|Asymmetric|
|-------------|-------|
|Same Key Used for `Encryption` and `Decryption` | There is **Public Key** for `Encryption` and **Private Key** for `Decryption`
Faster Than   Asymmetric but *Not Secure as Asymmetric*          | Slower Than Symmetric but *More Secure*       |
| **Example:** `AES` , `ChaCha20` , `RC4`| **Example:** `RSA`
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

using [PyCryptoDome]("https://pypi.org/project/pycryptodome/"):

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
