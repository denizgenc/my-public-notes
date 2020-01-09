Please note that the following are designed more for revision.

As such, they don't go into all the detail one might expect (or may even be required for the BCS
examination). I hope it is still useful regardless.

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
- Acts on continuous streams of data
- Changes each byte (or bit) to some other byte (or bit)

### Block ciphers
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
- A cryptographically secure pseudo-random number generator (CSPRNG) must satify:
  - Next bit test - can't predict the next bit in an arbitrary sequence of generated bits with a
    success of greater than 50%
  - State compromise extensions - can't guess past or future sequences in a stream if part of a
    generated stream is compromised
- CSPRNGs are used for generating keys
  - So the "initial state" for these functions are regarded as key material
