# Chinese Remainder Theorem (CRT) 🧩

---

## 🧠 Intuition

A legendary general in ancient China counted his soldiers by having them line up in rows: 3 at a time (2 left over), 5 at a time (3 left over), 7 at a time (2 left over). CRT finds the exact count.

CRT solves systems of simultaneous congruences:
```
x ≡ 2 (mod 3)
x ≡ 3 (mod 5)
x ≡ 2 (mod 7)
```
→ unique solution modulo 3×5×7=105: **x = 23**

**Mental model:** "Given remainders when dividing by several coprime moduli, find the unique number (up to their product) satisfying all simultaneously."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(k log N) — k congruences, each requires Extended GCD |
| Space Complexity | O(1) |
| Requirement | Moduli must be **pairwise coprime** (gcd(nᵢ, nⱼ) = 1 for all i ≠ j) |

---

## ⚙️ How It Works

Let `N = n₁ × n₂ × ... × nₖ` (product of all moduli).

For each congruence `x ≡ aᵢ (mod nᵢ)`:
1. Compute `Mᵢ = N / nᵢ` (product of all other moduli).
2. Compute `yᵢ = Mᵢ⁻¹ mod nᵢ` (modular inverse of Mᵢ modulo nᵢ, using Extended GCD).
3. The solution is: `x = (Σ aᵢ × Mᵢ × yᵢ) mod N`

**Intuition:** Each term `aᵢ × Mᵢ × yᵢ` contributes `aᵢ` when reduced mod `nᵢ` (since `Mᵢ × yᵢ ≡ 1 mod nᵢ`), and 0 when reduced mod `nⱼ` for j ≠ i (since nⱼ divides Mᵢ).

---

## 🔢 Step-by-Step Trace

`x ≡ 2 (mod 3), x ≡ 3 (mod 5), x ≡ 2 (mod 7)` → N = 105

| i | aᵢ | nᵢ | Mᵢ = N/nᵢ | yᵢ = Mᵢ⁻¹ mod nᵢ | aᵢ × Mᵢ × yᵢ |
|---|----|----|-----------|-----------------|--------------|
| 1 | 2  | 3  | 35        | 35⁻¹ mod 3 = 2  | 2×35×2 = 140 |
| 2 | 3  | 5  | 21        | 21⁻¹ mod 5 = 1  | 3×21×1 = 63  |
| 3 | 2  | 7  | 15        | 15⁻¹ mod 7 = 1  | 2×15×1 = 30  |

Sum = 140 + 63 + 30 = 233; 233 mod 105 = **23** ✅

Verify: 23 mod 3 = 2 ✓, 23 mod 5 = 3 ✓, 23 mod 7 = 2 ✓

---

## 🐍 Python Implementation

```python
from math import gcd

def extended_gcd(a, b):
    """Returns (g, x, y) such that a*x + b*y = g = gcd(a, b)."""
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    return g, y1, x1 - (a // b) * y1


def mod_inverse(a, m):
    """Compute a^(-1) mod m using Extended GCD. Raises if inverse doesn't exist."""
    g, x, _ = extended_gcd(a % m, m)
    if g != 1:
        raise ValueError(f"Inverse of {a} mod {m} doesn't exist (not coprime)")
    return x % m


def crt(remainders, moduli):
    """
    Solve system x ≡ remainders[i] (mod moduli[i]) for all i.
    Requirements: moduli must be pairwise coprime.
    Returns the unique solution x in [0, N) where N = product of moduli.
    """
    # Verify pairwise coprimality
    for i in range(len(moduli)):
        for j in range(i + 1, len(moduli)):
            assert gcd(moduli[i], moduli[j]) == 1, \
                f"Moduli {moduli[i]} and {moduli[j]} are not coprime"

    N = 1
    for n in moduli:
        N *= n                    # N = product of all moduli

    x = 0
    for a, n in zip(remainders, moduli):
        M = N // n                # Product of all OTHER moduli
        y = mod_inverse(M, n)     # Modular inverse of M modulo n
        x = (x + a * M * y) % N  # Add this term to the solution

    return x


# Example 1: Classic
print(crt([2, 3, 2], [3, 5, 7]))       # 23

# Example 2: Different values
print(crt([0, 3, 4], [3, 4, 5]))       # 39
# Verify: 39 mod 3=0 ✓, 39 mod 4=3 ✓, 39 mod 5=4 ✓
```

---

## 🎯 Recognize This Problem When...

- You need to find a number satisfying **multiple modular constraints simultaneously**.
- The problem gives remainders when dividing by several coprime numbers.
- Cryptography problems involving RSA optimization (CRT speeds up RSA decryption by 4×).
- Competitive programming: "find the smallest x such that x mod n₁ = r₁, x mod n₂ = r₂, ..."

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Multiple modular constraints, coprime moduli | ✅ Exact efficient solution |
| RSA decryption optimization | ✅ Factor N=pq, apply CRT per prime |
| Competitive programming modular reconstruction | ✅ Classic application |
| Moduli are NOT pairwise coprime | ❌ Generalized CRT needed (more complex) |
| Only one modular constraint | ❌ Simple modular arithmetic suffices |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [GCD](GCD.md) | Extended GCD computes the modular inverse needed |
| [Modular Exponentiation](ModularExponentiation.md) | Used inside RSA; CRT speeds up RSA decryption |
| [Fermat's Theorem](FermatTheorem.md) | Alternative for modular inverse when modulus is prime |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Chinese Remainder Theorem](https://www.hackerrank.com/challenges/chinese-remainder-theorem-2/problem) | HackerRank | 🟡 Medium |
| [Minimum Number of Days to Eat N Oranges](https://leetcode.com/problems/minimum-number-of-days-to-eat-n-oranges/) | LeetCode | 🔴 Hard |
| [Ugly Number III](https://leetcode.com/problems/ugly-number-iii/) | LeetCode | 🟡 Medium |
