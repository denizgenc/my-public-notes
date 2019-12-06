NCSC/Cisco Cryptography update, 2019-09-20  
Notes taken by Deniz Genç. I have attempted to be accurate but there may be mistakes, omissions,
alterations for brevity or clarity, or my own opinions below.  
Do not assume that the notes below reflect the views of anyone from NCSC, NCSC itself, or Cisco.

Quantum-safe Cryptography by Tim from NCSC
-------------------------
Today's talk covers NCSC's view on:

- Quantum threat to crypto
- Technical options for response
- Transition and standardisations
    - More info on all of these are available on NCSC's website:
      https://www.ncsc.gov.uk/whitepaper/quantum-safe-cryptography
      https://www.ncsc.gov.uk/whitepaper/quantum-key-distribution

Quantum Threat
-------------
- Public key cryptography underpins security of many modern networks
    - Confidentiality, e.g. RSA, Diffie-Hellman, ECDH
    - Integrity e.g. RSA-sign, DSA (Digital Signature Algorithm), ECDSA

Relies on the difficulty of hard problems

- Factorising integers (RSA)
- Discrete logarithms (DH, ECDH, DSA, ECDSA)

These problems are believed to be "infeasible" to solve using "conventional" computers.  
However, these problems could be solved "easily" using a large enough quantum computer.

- Quantum computers don't exist at a large enough scale today
- But they might do in the future
- NIST estimates that first cryptographically relevant quantum computer will be built around
  2030 for $1 billion
    - **It will be important to end reliance on quantum vulnerable PKC by this time**
    - If you require long-lived confidentiality, it wil be important to end reliance sooner
- Question: Is ~~D-Wave~~ A CERTAIN COMPANY creating large enough computers to attack PKC?
    - There are many different implementations of quantum computing
    - Each implementation has strengths and weaknesses when it comes to different problems; they
      can be seen as attacking a limited problem set
    - Current quantum computers may not be useful for breaking cryptography, but may have uses
      for other problems that are computationally difficult for conventional computers.

Symmetric cryptography
---------------------
In general, symmetric cryptography is not considered vulnerable to attack by quantum computers.  
The best attacks are infeasible provided that a large enough key is used

- For example, 256 bit key with AES is safe from either conventional or quantum computers
    - As an aside: quantum computers can run "Grover's algorithm" which could effectively halve
      key size - this doesn't scale efficiently

Technical Response to threat
--------------------
Two different approaches to quantum safe cryptography:

1. Quantum key distribution (QKD) - uses quantum phenomena. Requires new, specialised infrastructure
2. Post quantum cryptography (PQC) - new forms of crypto dependsing on the hardness of different
   mathematical problems. Can be implemented in hardware or software.

Based on current understanding, NCSC thinks PQC is more realistic.

Quantum key distro
---------------
Too immature to have confidence in its sustainability:

- Practical limitaions
    - Fundamentally point to point system - unclear how to use it for mobile comms, complex networks
      or IoT
- Unlikely to be cost effective
    - No generic hardware available for it at the moment
- Does not address whole security problem
    - Addresses confidentiality, but not integrity or availability.
- Security not fully understood
    - Comes with some strong claims, but...
    - ... it's quantum phenomena implemented with classic components - does this open up new attack
      vectors? We don't know

Post quantum crypto
-----------------
New public key crypto that is designed to be secure against attack with quantum computers  
Compatible with existing security architectures - we're just replacing one algorithm with another

Doesn't mean there aren't downsides:

- Lattice-based PQC: fast, but the keys are 1kB to 10kB
- Code based PQC: also really fast, but very big public keys (~1MB in some cases)
- Isogeny cryptography: keys about the same size as current PKC, but order of a magnitude slower in
  terms of computation

Standardisation and transition
-------------------
Main message: Don't jump too soon

Uncertainty:

- Which PQC system will offer best balance of efficiency and security?
- What are the most appropriate parametrisations and implementations?

Complex comms systems take a long time to develop and are difficult to upgrade.  
Unnecessary haste may be counter-prouctive.  
In general, NCSC recommends waiting for standardisation of new crypto and secure protocols (NCSC
  provides additional guidance for those with long lived classified info (CNI etc))

NIST Standardisation process
---------------------
NIST is running a public standardisation process for PQC algorithms.  

- Not a competition, they're looking for multiple suites for encryption etc.

Secure protocols are being considered in other bodies e.g. IETF

Timeline: 

- 2016 - Call for proposals
- 2017 - Submission deadline, round 1 algorithms announced
- 2018 - first PQC standardisation conference
- 2019 - Round 2 algorithms announced, and second PQC standardisation conference
- 2020 - Conclusions for Round 2, maybe another round

Expect some draft standards by 2022 to 2024.

Summary
-----
- Future quantum computers pose a threat to current PKC
- It will be important to end reliance on quantum vulnerable PKC by 2030 or even before for some
  use cases
- PQC is the most realistic option
- Don't jump too soon, wait for standardisation
- NIST is running a PQC standaridsation process. Draft standards expected 2022 - 2024

Question: Do you need to know physics to understand PQC?

- Yes, but some of the current discussions of the attacks and problems with quantum cryptography is
  about what we're doing now.
