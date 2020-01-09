Please note that the following are designed more for revision.

As such, they don't go into all the detail one might expect (or may even be required for the BCS
examination). For example, it won't describe how many steps/rounds a block cipher will go through,
etc. I hope it is still useful regardless.

# Definitions and concepts

## Types of cryptographic algorithms

### Symmetric
- Uses single key for encryption and decryption
- Faster than asymmetric encryption
- Key distribution is a problem
  - How do you transfer keys on the internet without it being compromised?
  - Number of keys required to secure connections between n people is on the order of n^2.

### Asymmetric
- Also called "public key cryptography / encryption"
- Uses two (or more?) keys for encryption and decryption
  - Either one can be used for encryption - the other must be used for decryption
  - Private key must be kept secret.
  - Public key made widely available.
- Slower than symmetric encryption

### Hash algorithms
- Creates fixed-length output from a variable input
  - Original data should not be recoverable
- Used to preserve integrity of data, not confidentiality
- Can also be used to generate "random" numbers deterministically, etc

### Stream ciphers
- A type of symmetric cipher
- Acts on continuous streams of data
- Changes each bit to another bit
- Uses the key provided to generate an "infinite" stream of digits to encrypt with - this is called
  the "keystream"

### Block ciphers
- A type of symmetric cipher
- Acts on fixed size blocks of plaintext (e.g. 128 bits - data must be padded if it does not match
  up) to output fixed size blocks of ciphertext
- Has different modes
  - ECB (Electronic Code Book)
    - only a key used on a block. Same plaintext block = same ciphertext block. This is flawed.
  - CBC (Cipher Block Chaining)
    - IV used on initial plaintext block, then each following plaintext block XORed with previous
      ciphertext block.
    - Fixes above flaw with ECB
  - CTR (Counter)
    - Nonce and counter block enciphered, then XORed with plaintext block. Counter is increased by
      one, then move on to next plaintext block.
    - Faster than CBC.
  - GCM (Galois Counter Mode)
    - Basically CTR mode, with added authentication. Means that you don't have to attach a separate
      MAC, which speeds things up.

## Kerckhoff's Principle
A cryptosystem should be secure even if everything about the system, except the key, is public
knowledge.
- i.e. must not rely on security through obscurity
- Reformulated by Claude Shannon in Shannon's Maxim: "the enemy knows the system"

## Random number generation / generators (RNG)
- A cryptographically secure pseudo-random number generator (CSPRNG) must satisfy:
  - Next bit test - can't predict the next bit in an arbitrary sequence of generated bits with a
    success of greater than 50%
  - State compromise extensions - can't guess past or future sequences in a stream if part of a
    generated stream is compromised
- CSPRNGs are used for generating keys
  - So the "initial state" for these functions are regarded as key material

# List of cryptographic algorithms
- [DES](#DES)
- [3DES](#3DES)
- [IDEA](#IDEA)
- [AES](#AES)
- [RC4](#RC4)
- [Salsa20](#Salsa20)
- [ChaCha20](#ChaCha20)
- [RSA](#RSA)
- [Diffie-Hellman](#Diffie-Hellman)
- [ECC](#ECC)
- [ECDH](#ECDH)
- [MD5](#MD5)
- [SHA-1](#SHA-1)
- [SHA-2](#SHA-2)
- [SHA-3](#SHA-3)
- [DSA](#DSA)
- [ECDSA](#ECDSA)

## DES
- Data Encryption Standard (FIPS 46, published 1977; withdrawn 2005)
- Symmetric encryption algorithm
  - Key size: 56 bits (+ 8 bit checksum)
- Block cipher
  - Block size: 64 bits
- No longer secure
  - Key is too short
  - First publically broken in 1997
- Replaced by 3DES

## 3DES
- Triple Data Encryption Standard (Published 1995)
- Symmetric encryption algorithm
  - Key size: up to 168 bits (three 56-bit keys)
- Block cipher
  - Block size: 64 bits
- Three ways you can use the keys:
  - Option 1: 3 different keys (168 bit key strength)
    - This option is the only one recommended by NIST for use until 2030
  - Option 2: 2 different keys (112 bit key strength)
  - Option 3: All keys are the same (56 bit key strength)
- Still used in some TLS cipher suites

## IDEA
- International Data Encryption Algorithm (1991)
- Symmetric encryption
  - Key size: 128 bits
- Block cipher
  - Block size: 64 bits
- Used to be patented. Patent expired in 2012

## AES
- Advanced Encryption Standard (first described 1998, standardised as FIPS 197 in 2001)
- Symmetric encryption algorithm
  - Key size: 128, 192 or 256 bits
- Block cipher
  - Block size: 128 bits

## RC4
- Rivest Cipher 4 (or alternatively Ron's Code 4). Trade secret, leaked 1994
- Symmetric encryption algorithm
  - Key size: 40 and 104 bits used in WEP (supports up to 2048 bits)
- Stream cipher
- Is weak when incorrectly used (see WEP). Not recommended.

## Salsa20
- Developed by Daniel J. Bernstein (published 2007)
- Symmetric cipher 
  - Key size: 128 or 256 bits
- Stream cipher
- Based on hashing
- No full attacks on it have been found so far

## ChaCha20
- Developed by Dan Bernstein, based on Salsa20 (published 2008)
- Symmetric cipher
  - Key size: 128 or 256 bits
- Stream cipher
- It's basically Salsa20 but better in terms of diffusion and speed
- Can be used in TLS
- Part of OpenSSH

## RSA
- Rivest Shamir Adelman (first described in 1977, released into the public domain in 2000)
- Asymmetric cipher
  - Key size: 2048 to 4096 bit keys recommended
- Security reliant on the difficulty of prime factorisation
  - At risk from quantum computing (Shor's algorithm)
- Needs large keys (compare with ECC)

## Diffie-Hellman
- First published 1976 (originally conceptualised by Ralph Merkle)
- Not an encryption algorithm - it is a key exchange method
- Uses asymmetric principles
  - Can't really speak of a single "key size" (see https://security.stackexchange.com/a/42562)
- Security reliant on the difficulty of the discrete logarithm problem
  - At risk from quantum computing (Shor's algorithm), like RSA
- Provides forward security
  - When used in ephemeral mode in TLS, provides perfect forward secrecy
- Main weakness is lack of authentication

## ECC
- Elliptic Curve Cryptography (first suggested 1985, started being widely used 2004 / 2005)
- Asymmetric principles
  - 228 bit key provides equivalent encryption to 2308 bit RSA key
- Security based on difficulty of elliptic curve discrete logarithm problem
- Strength also based on curve used
  - Curve25519 by Dan Bernstein is publically scrutinised curve, used in lots of applications

## ECDH
- Elliptic Curve Diffie Hellman
- Reduces the "key size" needed for Diffie-Hellman

## MD5
- Message Digest 5 (published 1992)
- Cryptographic hash algorithm
  - Hash length: 128 bits (32 hex digits)
- Very insecure, no longer recommended for security applications
  - Still used for checking file integrity
- Replaced by SHA-1

## SHA-1
- Secure Hash Algorithm 1 (published 1995, FIPS 180-4)
- Based on MD5's principles
- Cryptographic hash algorithm
  - Hash length: 160 bits (40 hex digits
- NIST recommends against SHA-1 if collision resistance is required
  - Security based on length of hash, hash not long enough any more
- Replaced by SHA-2 and SHA-3

## SHA-2
- Secure Hash Algorithm 2 (published 2001)
- Based on SHA-2
- Cryptographic hash algorithm
  - Hash length: 224, 256, 384, or 512 bits
- So far it is secure
  - Security is *still* based on length of hash, so may at one point be vulnerable
- Successor is SHA-3

## SHA-3
- Secure Hash Algorithm 3 (published 2015, based on Keccak)
- Cryptographic hash algorithm
  - Hash length: 224, 256, 384, 512, arbitrary
- Shares no mathematical basis with previous SHAs
- Decided by public competition
