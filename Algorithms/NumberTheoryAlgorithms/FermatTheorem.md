# Fermat's Little Theorem 🔑

---

## 🧠 Intuition

If `p` is prime and `a` is any integer not divisible by `p`, then raising `a` to the power `p-1` and taking mod p always gives 1:
```
a^(p-1) ≡ 1 (mod p)
```

This means `a^p ≡ a (mod p)` for all integers. It's a fundamental property of prime numbers.

**Why it matters:** This theorem gives us a fast way to compute **modular inverses** when the modulus is prime, and a quick (but imperfect) **primality test**.

**Mental model:** "In modular arithmetic with a prime modulus, every nonzero element raised to `p-1` equals 1. So `a^(p-2)` is the inverse of `a`."

---

## 📊 Complexity

| Operation | Time |
|-----------|------|
| Modular inverse via Fermat | O(log p) |
| Primality test (k rounds) | O(k log² p) |
| Space | O(1) |

---

## ⚙️ How It Works

**Application 1 — Modular Inverse:**
```
a × a^(p-2) = a^(p-1) ≡ 1 (mod p)
→ a^(-1) ≡ a^(p-2) (mod p)
```
Use `mod_pow(a, p-2, p)` — O(log p) via binary exponentiation.

**Application 2 — Fermat Primality Test:**
For a candidate prime `n`, pick random witnesses `a`:
- If `a^(n-1) mod n ≠ 1` → `n` is definitely **composite**.
- If `a^(n-1) mod n == 1` → `n` is **probably** prime (but may be a Carmichael number!).

---

## 🔢 Step-by-Step Trace

**Modular inverse of 3 mod 11 (p=11, prime):**
- Need: `3 × x ≡ 1 (mod 11)`
- Fermat: `x = 3^(11-2) mod 11 = 3^9 mod 11`
- `3^1=3, 3^2=9, 3^4=81 mod 11=4, 3^8=16 mod 11=5, 3^9=15 mod 11=4`
- So `3^9 mod 11 = 4`
- Verify: `3 × 4 = 12 ≡ 1 (mod 11)` ✅

---

## 🐍 Python Implementation

```python
def mod_inverse_fermat(a, p):
    """
    Compute modular inverse of a modulo p using Fermat's Little Theorem.
    REQUIRES: p must be PRIME and gcd(a, p) == 1.
    """
    return pow(a, p - 2, p)   # Python's built-in pow with 3 args uses fast exponentiation


def fermat_test(n, k=10):
    """
    Probabilistic primality test using Fermat's Little Theorem.
    k = number of witness rounds (higher k = lower false-positive probability).
    Returns: False → definitely composite; True → probably prime.
    WARNING: Fails on Carmichael numbers (e.g., 561 = 3×11×17).
    """
    import random
    if n < 2:
        return False
    if n in (2, 3, 5, 7):
        return True
    if n % 2 == 0:
        return False

    for _ in range(k):
        a = random.randrange(2, n - 1)
        if pow(a, n - 1, n) != 1:
            return False        # Fermat's condition violated → composite!

    return True   # Passed all tests → probably prime


# Examples — Modular Inverse
print(mod_inverse_fermat(3, 11))    # 4  (3 × 4 = 12 ≡ 1 mod 11)
print(mod_inverse_fermat(7, 13))    # 2  (7 × 2 = 14 ≡ 1 mod 13)

# Examples — Primality Test
print(fermat_test(17))              # True  (17 is prime)
print(fermat_test(15))              # False (15 = 3 × 5)
print(fermat_test(561))             # True  ← WRONG! 561 is Carmichael (composite)
# This is why Miller-Rabin is preferred in practice!
```

---

## 🎯 Recognize This Problem When...

- You need to compute a **modular inverse** and the modulus is **prime**.
- Problems involving **combinations/permutations mod prime**: `C(n,k) mod p = n! × (k!)^(-1) × ((n-k)!)^(-1) mod p`.
- Cryptography: RSA, Diffie-Hellman key exchange.
- You need a **quick primality check** and occasional false positives are acceptable.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Modular inverse with prime modulus | ✅ Simplest and fastest approach |
| Computing nCr mod prime in competitive programming | ✅ Classic application |
| Cryptographic operations with prime modulus | ✅ Industry standard |
| Modulus is NOT prime | ❌ Use Extended GCD for inverse |
| Reliable primality testing | ❌ Use Miller-Rabin (handles Carmichael numbers) |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [GCD](GCD.md) | Extended GCD works for any modulus (not just prime) |
| [Modular Exponentiation](ModularExponentiation.md) | How Fermat's inverse is computed: pow(a, p-2, p) |
| [Miller-Rabin](MillerRabin.md) | Stronger primality test; no Carmichael false positives |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Pow(x, n)](https://leetcode.com/problems/powx-n/) | LeetCode | 🟡 Medium |
| [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) | LeetCode | 🟡 Medium |
| Modular Inverse | [HackerRank](https://www.hackerrank.com/challenges/ncr-table/problem) | 🟡 Medium |
