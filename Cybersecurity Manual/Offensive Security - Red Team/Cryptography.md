**Ciphertext** - The result of encrypting a plaintext, encrypted data

**Cipher** - A method of encrypting or decrypting data. Modern ciphers are cryptographic, but there are many non cryptographic ciphers like Caesar.

**Plaintext** - Data before encryption, often text but not always. Could be a photograph or other file

**Encryption** - Transforming data into ciphertext, using a cipher.

**Encoding** - NOT a form of encryption, just a form of data representation like base64. Immediately reversible.

**Key** - Some information that is needed to correctly decrypt the ciphertext and obtain the plaintext.

**Passphrase** - Separate to the key, a passphrase is similar to a password and used to protect a key.

**Asymmetric encryption** - Uses different keys to encrypt and decrypt.

**Symmetric encryption** - Uses the same key to encrypt and decrypt

**Brute force** - Attacking cryptography by trying every different password or every different key

**Cryptanalysis** - Attacking cryptography by finding a weakness in the underlying maths

**Alice and Bob** - Used to represent 2 people who generally want to communicate. They’re named Alice and Bob because this gives them the initials A and B. [https://en.wikipedia.org/wiki/Alice_and_Bob](https://en.wikipedia.org/wiki/Alice_and_Bob) for more information, as these extend through the alphabet to represent many different people involved in communication.

**WARNING:** This room is very theory heavy. Cryptography is a big topic, and this room is designed to just scratch the surface.

--> It is used to protect confidentiality, provides integrity and ensure authenticity

--> Modulo is the key, unreversible

| Symetric | Asymetric |
|-----------|------------|
|DES, AES| RSA, ECC

## TOOLS 
- RSActf tool :  [https://github.com/Ganapati/RsaCtfTool](https://github.com/Ganapati/RsaCtfTool)f
- rsa tool : [https://github.com/ius/rsatool](https://github.com/ius/rsatool)

## RSA 

The key variables that you need to know about for RSA in CTFs are p, q, m, n, e, d, and c.

“p” and “q” are large prime numbers, “n” is the product of p and q.

The public key is n and e, the private key is n and d.

“m” is used to represent the message (in plaintext) and “c” represents the ciphertext (encrypted text).

P : large prime number 
Q : large prime number 
N : P * Q 
M : Plaintext message
C : ciphertext 

## Diffie-Hellman
Key exchange
