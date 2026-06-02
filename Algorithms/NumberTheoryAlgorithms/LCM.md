# Least Common Multiple (LCM) 🔁

---

## 🧠 Intuition

Traffic lights at an intersection: one cycles every 3 minutes, another every 4 minutes. When do they both turn green at the same time? Answer: every LCM(3, 4) = 12 minutes.

The LCM of two numbers is the **smallest positive integer divisible by both**. The elegant connection to GCD: `lcm(a, b) = |a × b| / gcd(a, b)`. Instead of searching for the LCM directly, we compute the GCD first (fast!) and divide.

**Mental model:** LCM(a, b) = "the smallest number that both a and b divide evenly into."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(log min(a, b)) — dominated by GCD |
| Space Complexity | O(1) |

---

## ⚙️ How It Works

**Formula:**
```
lcm(a, b) = |a × b| / gcd(a, b)
```

**For a list of numbers:** apply pairwise, left to right:
```
lcm(a, b, c) = lcm(lcm(a, b), c)
```

**Intuition for the formula:** Every prime factor appears in LCM to its **maximum** power across both numbers. Every prime factor appears in GCD to its **minimum** power. Since max + min = sum of both, multiplying a×b "double counts" each shared prime factor once, and dividing by GCD removes the double count.

---

## 🔢 Step-by-Step Trace

`lcm(12, 18)`:

1. `gcd(12, 18)`:
   - 18 = 1×12 + 6 → gcd(12, 6)
   - 12 = 2×6 + 0 → gcd = 6
2. `lcm = |12 × 18| / 6 = 216 / 6 = 36` ✅

Verify: 36 / 12 = 3 ✓, 36 / 18 = 2 ✓

`lcm([4, 6, 8])`:
- `lcm(4, 6) = 24/2 = 12`
- `lcm(12, 8) = 96/4 = 24`
- Result: 24 ✅

---

## 🐍 Python Implementation

```python
from math import gcd
from functools import reduce

def lcm(a, b):
    """Compute LCM of two integers."""
    if a == 0 or b == 0:
        return 0
    return abs(a * b) // gcd(a, b)    # Use // to avoid floating point


def lcm_list(nums):
    """Compute LCM of a list of integers."""
    return reduce(lcm, nums, 1)        # Start from 1 (identity for LCM)


# Note: Python 3.9+ has math.lcm built in!
# from math import lcm


# Examples
print(lcm(12, 18))          # 36
print(lcm(7, 5))            # 35
print(lcm(0, 5))            # 0

print(lcm_list([4, 6, 8]))  # 24
print(lcm_list([3, 5, 7]))  # 105 (all primes → product)
print(lcm_list([2, 4, 8]))  # 8 (8 already divisible by 2 and 4)
```

---

## 🎯 Recognize This Problem When...

- Two or more repeating events; find when they **synchronize** (next simultaneous occurrence).
- Need the **common denominator** for adding fractions.
- A period/cycle problem: "after how many steps do all processes realign?"
- You need to find the smallest number divisible by all elements in a set.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Synchronizing repeating events/cycles | ✅ LCM of all cycle lengths |
| Adding fractions (find common denominator) | ✅ Use LCM of denominators |
| Smallest multiple of a set of numbers | ✅ lcm_list gives the answer |
| Floating-point numbers | ❌ LCM is defined for integers only |
| Large numbers (a×b overflows) | ❌ Use big-integer arithmetic or compute gcd first |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [GCD](GCD.md) | LCM is computed using GCD: lcm = a×b / gcd(a,b) |
| [Modular Exponentiation](ModularExponentiation.md) | Both are fundamental number theory building blocks |
| [Chinese Remainder Theorem](ChineseRemainderTheorem.md) | CRT requires pairwise coprime moduli; LCM helps verify |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Least Common Multiple](https://www.hackerrank.com/challenges/between-two-sets/problem) | HackerRank | 🟢 Easy |
| [Ugly Number II](https://leetcode.com/problems/ugly-number-ii/) | LeetCode | 🟡 Medium |
| [Fraction Addition and Subtraction](https://leetcode.com/problems/fraction-addition-and-subtraction/) | LeetCode | 🟡 Medium |
