# Modular Exponentiation ⚡

---

## 🧠 Intuition

Computing `2^100` directly gives an astronomically large number. But if you only need `2^100 mod 1000`, you can take modulo at every multiplication step — the result stays small throughout.

The clever part: instead of multiplying `base` by itself `exp` times (O(exp) steps), use **repeated squaring**. Express `exp` in binary: `100 = 64 + 32 + 4 = 1100100₂`. Then `base^100 = base^64 × base^32 × base^4`. You compute `base^1, base^2, base^4, ..., base^64` by squaring each time — only log₂(exp) multiplications!

**Mental model:** "Square repeatedly (halving the exponent each time), multiply in the partial results where the exponent bit is 1."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(log exp) |
| Space Complexity | O(1) iterative |

---

## ⚙️ How It Works

**Algorithm (iterative):**
1. Initialize `result = 1`, `base = base % m`.
2. While `exp > 0`:
   - If `exp` is **odd** (least significant bit = 1): `result = (result × base) % m`.
   - Square: `base = (base × base) % m`.
   - Shift: `exp >>= 1` (divide by 2).
3. Return `result`.

```
base^13 where 13 = 1101₂:
bit0=1: result *= base^1, base → base^2
bit1=0: skip, base → base^4
bit2=1: result *= base^4, base → base^8
bit3=1: result *= base^8
result = base^1 × base^4 × base^8 = base^13 ✅
```

---

## 🔢 Step-by-Step Trace

`mod_pow(2, 10, 1000)` — compute 2^10 mod 1000

10 = `1010₂`

| Step | exp | exp odd? | result           | base             |
|------|-----|----------|------------------|------------------|
| Init | 10  | —        | 1                | 2                |
| 1    | 10  | No       | 1                | 2²=4             |
| 2    | 5   | Yes      | 1×4=4            | 4²=16            |
| 3    | 2   | No       | 4                | 16²=256          |
| 4    | 1   | Yes      | 4×256=1024%1000=24 | 256²=65536%1000=536 |
| 5    | 0   | —        | **24**           | —                |

`2^10 mod 1000 = 1024 mod 1000 = 24` ✅

---

## 🐍 Python Implementation

```python
def mod_pow(base, exp, m):
    """
    Compute (base^exp) % m efficiently using binary exponentiation.
    O(log exp) time, O(1) space.
    """
    if m == 1:
        return 0    # Everything mod 1 is 0

    result = 1
    base %= m       # Reduce base modulo m first

    while exp > 0:
        if exp & 1:                         # If current bit is 1
            result = (result * base) % m    # Multiply result by current base
        base = (base * base) % m            # Square the base
        exp >>= 1                           # Move to next bit

    return result


# Python's built-in pow(base, exp, m) does the same thing!
# It's implemented in C and should be preferred in production code.

# Examples
print(mod_pow(2, 10, 1000))      # 24    (2^10 = 1024, 1024 mod 1000 = 24)
print(mod_pow(7, 256, 13))       # 9
print(mod_pow(2, 1000000, 10**9 + 7))  # Some large number mod prime

# Modular inverse using Fermat's Little Theorem
# (only works when m is prime and gcd(a, m) = 1)
def mod_inverse(a, p):
    """Compute a^(-1) mod p using Fermat: a^(p-2) mod p (p must be prime)."""
    return mod_pow(a, p - 2, p)

print(mod_inverse(3, 11))        # 4  (because 3 × 4 = 12 ≡ 1 mod 11)
```

---

## 🎯 Recognize This Problem When...

- You need `(a^b) % m` where `b` is very large.
- Keywords: "compute power modulo", "modular inverse", "fast exponentiation".
- The problem involves **modular arithmetic** with large exponents (common in competitive programming).
- You need a **modular inverse** with a prime modulus (use `pow(a, p-2, p)`).
- Cryptographic operations (RSA encryption/decryption).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Compute large power modulo prime | ✅ O(log exp) — essential |
| Modular inverse (prime modulus) | ✅ `pow(a, p-2, p)` |
| Hashing with large polynomial | ✅ Used in Rabin-Karp |
| Modular inverse (non-prime modulus) | ❌ Use Extended GCD instead |
| Floating-point exponentiation | ❌ This is for integers only |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [GCD](GCD.md) | Extended GCD also computes modular inverse (works for any modulus) |
| [Fermat's Theorem](FermatTheorem.md) | Justifies the mod inverse formula a^(p-2) mod p |
| [Miller-Rabin](MillerRabin.md) | Uses modular exponentiation as its core operation |
| [Chinese Remainder Theorem](ChineseRemainderTheorem.md) | Uses modular inverse (via Extended GCD or Fermat) |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Pow(x, n)](https://leetcode.com/problems/powx-n/) | LeetCode | 🟡 Medium |
| [Super Pow](https://leetcode.com/problems/super-pow/) | LeetCode | 🟡 Medium |
| [Modular Exponentiation](https://www.hackerrank.com/challenges/functional-programming-warmups-in-recursion---scala/problem) | HackerRank | 🟢 Easy |
