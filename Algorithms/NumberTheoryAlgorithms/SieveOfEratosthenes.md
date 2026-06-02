# Sieve of Eratosthenes 🔢

---

## 🧠 Intuition

Ancient Greek method: write all numbers from 2 to N. Circle 2 (it's prime), then cross out all its multiples (4, 6, 8...). Move to the next uncrossed number (3 — it's prime), cross out its multiples (6, 9, 12...). Repeat. The circled numbers are all primes up to N.

The key optimization: start crossing out from `p²` (not `2p`), because all smaller multiples of `p` were already crossed out by smaller primes.

**Mental model:** "Mark the known non-primes, whatever's unmarked is prime."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n log log n) |
| Space Complexity | O(n) |

> Nearly linear in practice. Much faster than testing each number individually (which would be O(n√n)).

---

## ⚙️ How It Works

1. Create `is_prime[0..n]`, all set to `True`.
2. Set `is_prime[0] = is_prime[1] = False` (0 and 1 are not prime).
3. For each `i` from 2 to `√n`:
   - If `is_prime[i]` is True (i is prime):
     - Mark all multiples of `i` starting from `i²` as `False`.
     - (Multiples `2i, 3i, ..., (i-1)i` were already marked by smaller primes.)
4. Collect all indices where `is_prime[i] == True`.

---

## 🔢 Step-by-Step Trace

Finding primes up to 20:

```
Start:   [2,3,4,5,6,7,8,9,10,11,12,13,14,15,16,17,18,19,20]

i=2 (prime): mark 4,6,8,10,12,14,16,18,20
After:   [2,3,5,7,9,11,13,15,17,19]

i=3 (prime): mark 9,12,15,18 (12,18 already marked)
After:   [2,3,5,7,11,13,17,19]

i=4: not prime, skip
√20 ≈ 4.5 → stop

Primes: [2, 3, 5, 7, 11, 13, 17, 19]
```

---

## 🐍 Python Implementation

```python
def sieve(n):
    """
    Find all primes up to n (inclusive) using Sieve of Eratosthenes.
    Returns a sorted list of primes.
    """
    if n < 2:
        return []

    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False   # 0 and 1 are not prime

    for i in range(2, int(n ** 0.5) + 1):   # Only need to sieve up to √n
        if is_prime[i]:
            # Mark all multiples of i starting from i² as composite
            for j in range(i * i, n + 1, i):
                is_prime[j] = False

    return [i for i in range(n + 1) if is_prime[i]]


def sieve_with_smallest_prime_factor(n):
    """
    Enhanced sieve that also stores the smallest prime factor of each number.
    Enables O(log n) factorization of any number up to n.
    """
    spf = list(range(n + 1))    # spf[i] = smallest prime factor of i

    i = 2
    while i * i <= n:
        if spf[i] == i:          # i is prime (its own smallest prime factor)
            for j in range(i * i, n + 1, i):
                if spf[j] == j:  # Not yet marked
                    spf[j] = i
        i += 1

    def factorize(x):
        """Factorize x in O(log x) using the spf table."""
        factors = []
        while x > 1:
            factors.append(spf[x])
            x //= spf[x]
        return factors

    return spf, factorize


# Examples
print(sieve(30))
# [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]

print(sieve(2))
# [2]

spf, factorize = sieve_with_smallest_prime_factor(100)
print(factorize(60))   # [2, 2, 3, 5]
print(factorize(97))   # [97] — it's prime
```

---

## 🎯 Recognize This Problem When...

- You need to generate **all primes up to a bound** N.
- You have **many primality queries** on numbers ≤ N (pre-compute once, answer in O(1)).
- Competitive programming problems with "count primes", "sum of primes", "prime factorization × many numbers".
- You need the **smallest prime factor** of every number up to N for fast factorization.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Generate all primes up to N | ✅ Fastest method |
| Many primality queries on small numbers | ✅ Pre-compute, then O(1) query |
| Find prime factors of many numbers | ✅ Use SPF sieve variant |
| Single primality test for very large number | ❌ Use Miller-Rabin instead |
| N > ~10⁸ (memory constraint) | ❌ Use segmented sieve (O(√n) memory) |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Miller-Rabin](MillerRabin.md) | Single-number primality test for large numbers |
| [GCD](GCD.md) | Often used together for number theory problems |
| [Modular Exponentiation](ModularExponentiation.md) | Combines with sieve in cryptographic applications |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Count Primes](https://leetcode.com/problems/count-primes/) | LeetCode | 🟡 Medium |
| [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/) | LeetCode | 🟡 Medium |
| [Prime Factorization](https://www.hackerrank.com/challenges/euler254/problem) | HackerRank | 🟡 Medium |
