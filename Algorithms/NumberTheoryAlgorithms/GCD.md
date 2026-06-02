# Greatest Common Divisor (GCD) 🔢

---

## 🧠 Intuition

Imagine two gears: one with 48 teeth, one with 18. The largest gear tooth spacing that fits evenly into both is the GCD. Euclid noticed that the GCD of two numbers is the same as the GCD of the smaller number and the remainder when the larger is divided by the smaller — so you can keep shrinking the problem until one side hits zero.

**Mental model:** "Keep dividing — the GCD survives every remainder step, and the last nonzero remainder is the answer."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time   | O(log min(a, b)) |
| Space  | O(1) iterative / O(log min(a, b)) recursive call stack |

---

## ⚙️ How It Works

The algorithm is based on **Euclid's identity**:

```
gcd(a, b) = gcd(b, a mod b)
gcd(a, 0) = a
```

1. If `b` is 0, return `a` (base case).
2. Otherwise, replace `(a, b)` with `(b, a mod b)`.
3. Repeat until `b` reaches 0.
4. The current value of `a` is the GCD.

The **Extended Euclidean Algorithm** additionally finds integers `x` and `y` such that:
```
a·x + b·y = gcd(a, b)    (Bezout's identity)
```
This is the foundation of modular inverses.

---

## 🔢 Step-by-Step Trace

Computing `gcd(48, 18)`:

| Step | a  | b  | a mod b |
|------|----|----|---------|
| 1    | 48 | 18 | 12      |
| 2    | 18 | 12 | 6       |
| 3    | 12 | 6  | 0       |
| 4    | 6  | 0  | —       |

b = 0, so **gcd(48, 18) = 6**.

Computing `extended_gcd(30, 18)` — back-substitution:

| Call              | Returns (g, x, y)  | Meaning             |
|-------------------|--------------------|---------------------|
| extended_gcd(18,12) | (6, 1, -1)       | 18·1 + 12·(-1) = 6  |
| extended_gcd(30,18) | (6, -1, 2)       | 30·(-1) + 18·2 = 6  |

---

## 🐍 Python Implementation

```python
def gcd(a, b):
    # Iterative Euclidean algorithm — O(log min(a,b)), O(1) space
    while b:
        a, b = b, a % b   # replace (a,b) with (b, a mod b)
    return a              # when b==0, a holds the GCD

def gcd_recursive(a, b):
    # Recursive version — identical logic, cleaner for small inputs
    return a if b == 0 else gcd_recursive(b, a % b)

def extended_gcd(a, b):
    """
    Returns (g, x, y) such that a*x + b*y = g = gcd(a, b).
    Used to compute modular inverses.
    """
    if b == 0:
        return a, 1, 0               # base case: gcd(a,0)=a, coefficients (1,0)
    g, x1, y1 = extended_gcd(b, a % b)
    # Back-substitute: x = y1, y = x1 - floor(a/b)*y1
    return g, y1, x1 - (a // b) * y1

# --- Examples ---
print(gcd(48, 18))             # 6
print(gcd_recursive(100, 75))  # 25

g, x, y = extended_gcd(30, 18)
print(g, x, y)                 # 6 -1 2  →  30·(-1) + 18·2 = 6
```

---

## 🎯 Recognize This Problem When...

- The problem asks for the **largest divisor** shared by two or more numbers
- You need to **simplify a fraction** (divide numerator and denominator by GCD)
- You need a **modular inverse** (use Extended GCD)
- You see phrases like "pairwise coprime", "relatively prime", or need to verify `gcd == 1`
- The problem involves **LCM** (always computed via GCD)
- Cryptographic operations (RSA key generation, Diffie-Hellman)

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Finding largest common divisor of two numbers | ✅ Core use case |
| Simplifying fractions | ✅ Divide both by gcd(numerator, denominator) |
| Computing LCM | ✅ Use `lcm = a * b // gcd(a, b)` |
| Finding modular inverse (prime modulus) | ✅ Use Extended GCD or Fermat |
| Checking if two numbers are coprime | ✅ Check if `gcd(a, b) == 1` |
| Finding GCD of a list of numbers | ✅ Reduce pairwise: `gcd(gcd(a,b), c)` |
| Primality testing | ❌ Use Sieve or Miller-Rabin instead |
| Working with floating-point numbers | ❌ GCD is defined for integers only |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [LCM](./LCM.md) | Computed directly from GCD: `lcm = a·b // gcd(a,b)` |
| [Modular Exponentiation](./ModularExponentiation.md) | Used to compute modular inverses via Fermat's theorem (requires prime modulus) |
| [Chinese Remainder Theorem](./ChineseRemainderTheorem.md) | Uses Extended GCD internally to find modular inverses |
| [Fermat's Little Theorem](./FermatTheorem.md) | Fermat-based inverse only works for prime moduli; Extended GCD is more general |
| [Miller-Rabin](./MillerRabin.md) | Miller-Rabin uses modular arithmetic; GCD is used as a quick composite check |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Find Greatest Common Divisor of Array](https://leetcode.com/problems/find-greatest-common-divisor-of-array/) | LeetCode | 🟢 Easy |
| [Greatest Common Divisor of Strings](https://leetcode.com/problems/greatest-common-divisor-of-strings/) | LeetCode | 🟢 Easy |
| [Fraction Addition and Subtraction](https://leetcode.com/problems/fraction-addition-and-subtraction/) | LeetCode | 🟡 Medium |
| [Ugly Number III](https://leetcode.com/problems/ugly-number-iii/) | LeetCode | 🟡 Medium |
| [GCD Sort of an Array](https://leetcode.com/problems/gcd-sort-of-an-array/) | LeetCode | 🔴 Hard |
