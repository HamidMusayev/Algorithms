# Miller-Rabin Primality Test 🔬

---

## 🧠 Intuition

Fermat's test fails on **Carmichael numbers** (composites that pass Fermat for every witness). Miller-Rabin is stronger: it uses an additional "square root of 1 mod n" check that Carmichael numbers cannot fool.

The key insight: for any prime `p`, the only square roots of 1 mod p are ±1. If during the squaring process we find `x² ≡ 1 (mod n)` but `x ≢ ±1 (mod n)`, then `n` is definitely composite.

**Mental model:** "Fermat test + check that 1's square roots are ±1. Composites almost always fail one of these."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Per witness | O(log³ n) |
| k witnesses | O(k log³ n) |
| Space | O(1) |
| Deterministic for n < 3.3×10²⁴ | ✅ With fixed witness set |

---

## ⚙️ How It Works

Write `n - 1 = 2^r × d` where `d` is odd.

For witness `a`:
1. Compute `x = a^d mod n`.
2. If `x == 1` or `x == n - 1` → **probably prime** (continue to next witness).
3. Repeat `r - 1` times: square `x = x² mod n`.
   - If `x == n - 1` → **probably prime** (break inner loop).
   - If `x == 1` (without seeing -1 first) → **composite**.
4. If inner loop finishes without finding `n - 1` → **composite**.

Using the deterministic witness set `{2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37}` is correct for all `n < 3.3 × 10²⁴`.

---

## 🔢 Step-by-Step Trace

Test `n = 561` (Carmichael number — Fermat passes but Miller-Rabin catches it):

`n - 1 = 560 = 2^4 × 35`, so `r = 4, d = 35`

Witness `a = 2`:
- `x = 2^35 mod 561 = 263`
- `x ≠ 1` and `x ≠ 560`
- Square: `263² mod 561 = 166`, ≠ 560
- Square: `166² mod 561 = 67`, ≠ 560
- Square: `67² mod 561 = 1` ← `x = 1` but we never saw `x = 560` → **COMPOSITE** ✅

---

## 🐍 Python Implementation

```python
def miller_rabin(n, witnesses=(2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37)):
    """
    Deterministic primality test for n < 3.3×10^24.
    Returns True if n is prime, False if composite.
    """
    if n < 2:
        return False
    # Quick checks for small primes and even numbers
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for p in [2, 3, 5, 7, 11, 13, 17, 19, 23, 29, 31, 37]:
        if n == p:
            return True
        if n % p == 0:
            return False

    # Write n-1 = 2^r * d (d must be odd)
    d, r = n - 1, 0
    while d % 2 == 0:
        d //= 2
        r += 1

    for a in witnesses:
        if a >= n:
            continue

        x = pow(a, d, n)            # Compute a^d mod n

        if x == 1 or x == n - 1:    # Trivial square root → continue
            continue

        # Square up to r-1 times looking for -1
        for _ in range(r - 1):
            x = (x * x) % n
            if x == n - 1:
                break               # Found -1 → this witness passes
        else:
            return False            # Never found -1 → composite!

    return True   # All witnesses passed → prime


# Examples
print(miller_rabin(2))            # True
print(miller_rabin(17))           # True
print(miller_rabin(561))          # False (Carmichael — Fermat would say True!)
print(miller_rabin(1_000_000_007))# True (common prime in competitive programming)
print(miller_rabin(10**18 + 9))   # True (large prime)
print(miller_rabin(4))            # False
print(miller_rabin(15))           # False
```

---

## 🎯 Recognize This Problem When...

- You need to test **primality of large numbers** (> 10⁶) efficiently.
- Keywords: "is n prime?", "find prime factors", "next prime after n".
- Cryptographic contexts (RSA key generation requires finding large primes).
- Competitive programming with n up to 10¹⁸ or larger.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Single large number primality test | ✅ O(log³ n) — much faster than trial division |
| All primes up to n | ❌ Use Sieve of Eratosthenes |
| Cryptographic prime generation | ✅ Standard approach |
| Numbers < 10⁶ | ❌ Sieve + lookup is faster |
| Need 100% certainty | ✅ Use deterministic witness set for 64-bit ints |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Fermat's Theorem](FermatTheorem.md) | Weaker version; fails on Carmichael numbers |
| [Sieve of Eratosthenes](SieveOfEratosthenes.md) | Generate all primes up to N (different use case) |
| [Modular Exponentiation](ModularExponentiation.md) | Core subroutine: pow(a, d, n) |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Count Primes](https://leetcode.com/problems/count-primes/) | LeetCode | 🟡 Medium |
| [Prime Palindrome](https://leetcode.com/problems/prime-palindrome/) | LeetCode | 🟡 Medium |
| [Miller-Rabin](https://www.hackerrank.com/challenges/primality-test/problem) | HackerRank | 🟡 Medium |
