---
layout: project
title: The First 100,000 Primes
image: /images/primes/cover.png
summary: A Book of numbers
---

I've always liked books of numbers: logarithms, phone, random, I dont care.

I decided I wanted a book that contained lots of prime numbers, partly because I don't really understand much about them, but felt I should, and that this would help.

![](/images/primes/cover.png)

First, I need a large list of primes. Python made that pretty easy.
```python
import pprint
# Initialize a list
primes = []
for possiblePrime in range(2, 2000000):  # a guess - more than enough
    # Assume number is prime until shown it is not.
    isPrime = True
    for num in range(2, int(possiblePrime ** 0.5) + 1):
        if possiblePrime % num == 0:
            isPrime = False
            break

    if isPrime:
        primes.append(possiblePrime)
    if len(primes) == 100000:
        # we have enough
        break

pprint.pprint(len(primes))
pprint.pprint(primes)
```

I piped that into a text document, and formatted it in vim

![](/images/primes/pages.png)

# Thoughts
Lots of fun. Took less than 2 hours from idea to manuscript.

I haven't learned much more about primes from this project, but I give this book away at Xmas and get lots of confused looks, which makes it all worthwhile.
