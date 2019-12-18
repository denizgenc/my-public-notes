Clues provided:
- Keypad code is 4 digits long
- Keypad code had one digit repeated
- The code is a prime number
- You can figure out what digits were used by looking at the keypad

Looking at the keypad, the numbers 1, 3, and 7 are most worn (insert screenshot?)

So need to generate a 4 digit number where one of the digits is repeated, but that's it. Here's the
code I came up with in Python in the REPL:

```python
from itertools import permutations
nums = ['1','3','7']
res = set() # Allows us to get rid of repeats for free

for i in nums:
  for j in permutations(nums): # All possible orderings of the 3 numbers
    k = list(j) # Need to cast these orderings to a list to "add" to it
    res.add( int("".join([i] + k))) # Combine one of the characters with the others, join as a
    res.add( int("".join(k[:1] + [i] + k[1:]))) # single string, cast to int, then add to the set
    res.add( int("".join(k[:2] + [i] + k[2:])))
    res.add( int("".join(k + [i])))
```

The result looked good enough. Now I needed a primality test - I just nicked this one off the
Wikipedia article for it: https://en.wikipedia.org/wiki/Primality\_test#Pseudocode

```python
def is_prime(n):
  if n < 4:
    return n > 1
  if n % 2 == 0 or n % 3 == 0:
    return False
  i = 5
  while i ** 2 <= n:
    if n % i == 0 or n % (i + 2) == 0:
      return False
    i += 6
  return True
```

And so, I get a nice little list of valid codes at the end of it:

```python
>>> for i in res:
...     if is_prime(i):
...             print(i)
...
7331
3371
3137
1733
1373
```

No 1337 (it has 7 and 191 as factors), but I guess that would be the first one people would try.
