Cryptography @ GDS, 2019-08-14

1st rule of crypto - don't roll your own
  - Don't design your own primitives, don't implement other people's primitives yourself (even if
    you think you understand them)

Another rule is that you shouldn't consider primitves you're using as secure unless they've been
widely reviewed by the community
- A primitive is a building block for a crypto system

Cryptographic hashes:

- md5, sha1, sha-256, blake2, ripemd
- A hash is a function that maps an arbitrary binary input to a fixed-length, pseudo-random
  output
    - pseudo-random = randomness that is "predictable", that is generated deterministically
- A **cryptographic** hash function has extra properties that make it useful in a security
  context
- Because *all* inputs are put into a fixed length output, there *must* be collisions
    - Example: checking file integrity
    - Assume that Alice and Bob have a secure communication channel
    - Alice wants to send a file to Bob, and wants to be sure of the integrity of the file (and
      it's a big file, so can't re-transmit via the secure channel)
    - Bob generates a hash, and then tells Alice via the secure channel.
- Play around with this: `sha256sum` (Linux) or `shasum -a 256 -p` (MacOS)
    - hash of "Hello": 66a045b452102c59d840ec097d59d9467e13a3f34f6494e539ffd32c1bb35f18
    - hash of "Hellp": b470581eb793e944838314f87926f3f6324e11f8d156014aefce1bcdb3720964
        (note these are technically the hashes of "Hell(o|p)" with a newline at the end)
- How do you decide a good hash?
    - Performance?
        - You *don't* want it to be performant. Logging in, changing passwords etc don't take
          too long. However, an attacker may need to crunch a *lot* of numbers - so let's make
          their job as difficult as possible - using more complex hashes is better!
        - Use specific algorithms that are really difficult to optimise (bcrypt, scrypt)
        - Or, use key stretching, which is just hashing it over and over again. This is nice
          because it's arbitrarily scalable, and you can apply it retroactively to existing
          password hashes in your database.
            - Key stretching and salting is usually combined into a single function called
              a "key derivation function", an example of which is PBKDF2
    - A hash that reduces the possibility of collisions (especially malicious collisions)
        - Eve wants to create a file that outputs the same file that Alice wants to send to Bob.
        - We want to make it hard ("computationally infeasible") to do this, or at least to
          make a file with a collision that has anything that isn't just random data.
            - How do you make a valid PDF, JPEG, etc while colliding with an existing PDF etc?
            - Answer: "comment" sections in PDF etc that allow you to fill the file with
              arbitrary data
        - Second preimage resistance
            - Have messages m\_1 and m\_2, and a hash value H
            - Second preimage is a property where if m\_1 is given, and m\_2 is a free
              variable (and hash H is an implicit given)
    - Avalanche effect
        - If a similar input produces a similar hash, that means that an attacker can narrow
          down their field of search for an input that will collide with another one.
        - To prevent that from happening, a good cryptographic hash will cause an avalanche
          effect so that even incredibly small changes result in a very different hash.
        - (Formally: 1 bit change in input causes 50% of bits to flip in hash)
    - What if Eve is a package author which is a backup application, but wants to trick someone
      into downloading a version of the package that sends Eve a copy of the backed up files?
        - The package's code is clean since it's been audited
        - Previously Eve had to find a string that would collide with a specific other message. 
          Now she has to find any two strings that have the same hash
        - This is a birthday attack (because it is based on the birthday paradox - collision
          probabilities increases on the order of n^2)
        - If you have a hash that is resistant to birthday attacks, it is called "collision
          resistant"
    - Collision Resistance is where messages m\_1 and m\_2 are free variables, and the hash is
      also a free variable
        - It's a superset of Second Preimage Resistance
    - If Eve has access to a database of hashed passwords, if she has a list of the 10 million
      most common passwords in the world, she can run the same hashing algorithm on those common
      passwords, and then check if there are any matches with the database
        - You could use an obscure hashing algorithm
        - Or you can add a *unique* salt to each hashed value. A salt makes it so that an
          attacker would need to generate a new rainbow table for each and every row in your
          password hash database
        - Adding a salt enables preimage resistance
    - Preimage Resistance is where m\_1 is a free variable, m\_2 doesn't exist/is not relevant,
      and H is a given
        - This is the most basic thing you want from a cryptographic hash
- When hashes are broken, they are broken gradually
    - First the collision resistance goes, then 2nd preimage resistance, then preimage
    - Understand what your use of these hashes rely on, so you know when to panic
        - SHA-1's collision resistance is kind of gone, but that's not really a problem for git.
          However, if second preimage resistance goes, *then* it's a problem.
- Message Authentication Codes (MAC)
    - Hashed(message + shared secret) = MAC
        - (don't actually concatenate the message with the secret - this can enable length
          extension attacks - use HMAC)
    - By checking for the MAC, both parties communicating don't need to know what the message
      was to verify that each other got the same message
    - You *could* use these instead of just a plain hash of a file to verify that the Ubuntu ISO
      you're downloading is legit, but you'd have to publish the shared secret. However, someone
      with the shared secret could also generate their own MACs to make their evil Ubuntu ISOs
      seem legit.
        - This is because shared secrets = keys for symmetric cryptography. Everything we have
          written above has to do with symmetric encryption.

- Replay attacks are possible when the info you've signed isn't insufficiently bound to the time or
  the context of the thing you're signing

