# PyElliptic

PyElliptic is a high level wrapper for the cryptographic library : OpenSSL.
Under the GNU General Public License

Python3 compatible. For GNU/Linux and Windows.
Require OpenSSL

## Features

### Asymmetric cryptography using Elliptic Curve Cryptography (ECC)

* Key agreement : ECDH
* Digital signatures : ECDSA
* Hybrid encryption : ECIES (like RSA)

### Symmetric cryptography

* AES-128 (CFB and CBC)
* AES-256 (CFB and CBC)
* Blowfish (CFB and CBC)

### Other

* CSPRNG
* HMAC (using SHA512)

## Example

```python
#!/usr/bin/python

import pyelliptic

# Symmetric encryption
iv = pyelliptic.cipher.gen_IV('aes-256-cfb')
ctx = pyelliptic.cipher("secretkey", iv, 1, ciphername='aes-256-cfb')

ctx.update('test1')
ctx.update('test2')
ciphertext = ctx.final()

ctx2 = pyelliptic.cipher("secretkey", iv, 0, ciphername='aes-256-cfb')
print ctx2.ciphering(ciphertext)

# Asymmetric encryption
alice = pyelliptic.ecc() # default curve: sect283r1
bob = pyelliptic.ecc(curve='sect571r1')

ciphertext = alice.encrypt("Hello Bob", bob.get_pubkey())
print bob.decrypt(ciphertext)

signature = bob.sign("Hello Alice")
# alice's job :
print pyelliptic.ecc(pubkey=bob.get_pubkey()).verify(signature, "Hello Alice")

# ERROR !!!
try:
    key = alice.get_ecdh_key(bob.get_pubkey())
except: print("For ECDH key agreement, the keys must be defined on the same curve !")

alice = pyelliptic.ecc(curve='sect571r1')
print alice.get_ecdh_key(bob.get_pubkey()).encode('hex')
print bob.get_ecdh_key(alice.get_pubkey()).encode('hex')
```