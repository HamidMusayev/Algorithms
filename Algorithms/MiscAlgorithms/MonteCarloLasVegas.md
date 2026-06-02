# Monte Carlo & Las Vegas Algorithms 🎲

---

## 🧠 Intuition

**Two distinct types of randomized algorithms:**

**Monte Carlo:** You flip fast and accept a small chance of being wrong.
- Example: survey 1000 random voters to estimate election outcome. Fast, but occasionally wrong.

**Las Vegas:** You may take unpredictable time, but the answer is ALWAYS correct.
- Example: randomly shuffle a deck, check if it's sorted. Repeat until you confirm. Always correct, but time varies.

**Mental model:**
- Monte Carlo = "Fast but might be wrong (with bounded probability)"
- Las Vegas = "Always correct but time is random"

---

## 📊 Complexity

| Algorithm Type | Correctness | Time |
|----------------|-------------|------|
| **Monte Carlo** | Probabilistic (ε error) | Deterministic / Fixed |
| **Las Vegas** | Always correct | Random (expected) |
| **Deterministic** | Always correct | Fixed worst case |

---

## ⚙️ How It Works

### Monte Carlo

**Core idea:** Use randomness to approximate an answer, with provable error bounds.

Key examples:
- **π estimation** (random points in circle)
- **Miller-Rabin primality test** (fails only on Carmichael numbers — negligible probability)
- **Randomized algorithms in ML** (random forests, SGD with random batches)

**Error analysis:** Run k independent rounds. Probability of being wrong in ALL k rounds = ε^k (exponentially small). Choose k to hit your tolerance.

### Las Vegas

**Core idea:** Use randomness to guide search, but verify before accepting.

Key examples:
- **Randomized QuickSort** (random pivot → O(n log n) expected, always correct)
- **Randomized hash functions** (retry on collision, always finds a slot)
- **Randomized backtracking** (try random choices, backtrack if stuck)

---

## 🔢 Step-by-Step Trace

**Monte Carlo — Estimating π:**

Drop 1,000,000 random points in [0,1] × [0,1]. Count those inside the unit circle (x²+y²≤1). Ratio ≈ π/4. So π ≈ 4 × (inside/total).

With 1M points, error is typically < 0.001.

**Las Vegas — Randomized QuickSort:**

Array: `[3, 1, 4, 1, 5, 9, 2, 6]`
- Pick random pivot (say, 4). Partition: `[3,1,1,2] | 4 | [5,9,6]`.
- Always correct. Expected O(n log n) regardless of input order.

---

## 🐍 Python Implementation

```python
import random
import math


# ── MONTE CARLO ──────────────────────────────────────────────────────────────

def estimate_pi(num_samples=1_000_000):
    """
    Monte Carlo estimation of π.
    Drops random points in unit square, counts those inside unit circle.
    Error ≈ O(1/√n) — needs millions of samples for good precision.
    """
    inside = 0
    for _ in range(num_samples):
        x = random.random()
        y = random.random()
        if x * x + y * y <= 1.0:
            inside += 1
    return 4 * inside / num_samples


def monte_carlo_integration(f, a, b, num_samples=100_000):
    """
    Estimate ∫[a,b] f(x) dx using Monte Carlo integration.
    Samples random x values and averages f(x).
    """
    total = sum(f(random.uniform(a, b)) for _ in range(num_samples))
    return (b - a) * total / num_samples


# ── LAS VEGAS ────────────────────────────────────────────────────────────────

def quicksort_lv(arr):
    """
    Las Vegas Randomized QuickSort.
    Always correct. Expected O(n log n) — no bad input can force O(n²).
    """
    if len(arr) <= 1:
        return arr
    pivot = random.choice(arr)           # Random pivot — key to Las Vegas property
    left   = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]
    right  = [x for x in arr if x > pivot]
    return quicksort_lv(left) + middle + quicksort_lv(right)


def random_permutation(arr):
    """
    Fisher-Yates shuffle — Las Vegas style.
    Always produces a uniformly random permutation in O(n).
    """
    arr = arr.copy()
    n = len(arr)
    for i in range(n - 1, 0, -1):
        j = random.randint(0, i)           # Random index ≤ i
        arr[i], arr[j] = arr[j], arr[i]   # Swap
    return arr


# ── EXAMPLES ─────────────────────────────────────────────────────────────────

# Monte Carlo
print(f"π ≈ {estimate_pi(1_000_000):.5f}")   # e.g., 3.14159

# Monte Carlo integration of x² from 0 to 1 (exact = 1/3)
result = monte_carlo_integration(lambda x: x**2, 0, 1, 100_000)
print(f"∫x² dx ≈ {result:.4f}")              # ≈ 0.3333

# Las Vegas QuickSort
arr = [5, 2, 8, 1, 9, 3, 7, 4, 6]
print("Sorted:", quicksort_lv(arr))          # [1, 2, 3, 4, 5, 6, 7, 8, 9]
```

---

## 🎯 Recognize This Problem When...

**Monte Carlo:**
- Exact computation is infeasible but approximation is acceptable.
- Keywords: "estimate", "approximate", "simulate", "probability".
- The error can be bounded and made arbitrarily small with more samples.

**Las Vegas:**
- Exact answer required but randomness can speed up the algorithm.
- Keywords: "randomized", "expected time", "with high probability".
- Deterministic algorithm exists but is too slow (random pivot avoids worst case).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Vegas/Carlo | Verdict |
|-----------|-------------|---------|
| Approximate numerical integration | Monte Carlo | ✅ |
| Primality test (large numbers) | Monte Carlo | ✅ (Miller-Rabin) |
| Sorting (avoiding O(n²) worst case) | Las Vegas | ✅ (Random pivot) |
| Exact answer needed, can't approximate | Monte Carlo | ❌ |
| Fixed deadline (can't retry) | Las Vegas | ❌ |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Miller-Rabin](../NumberTheoryAlgorithms/MillerRabin.md) | Classic Monte Carlo primality test |
| [Karger's Algorithm](KargerAlgorithm.md) | Monte Carlo min-cut |
| [Reservoir Sampling](ReservoirSampling.md) | Las Vegas-style: always uniformly correct |
| [Quick Sort](../SortAlgorithms/QuickSort.md) | Randomized pivot makes it Las Vegas |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Random Pick Index](https://leetcode.com/problems/random-pick-index/) | LeetCode | 🟡 Medium |
| [Shuffle an Array](https://leetcode.com/problems/shuffle-an-array/) | LeetCode | 🟡 Medium |
| [Monte Carlo Simulation](https://www.hackerrank.com/challenges/dice-path/problem) | HackerRank | 🟡 Medium |
