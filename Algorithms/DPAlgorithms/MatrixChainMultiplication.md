# Matrix Chain Multiplication 🔢

---

## 🧠 Intuition

Matrix multiplication is associative: `(AB)C = A(BC)`, but the number of scalar operations can differ wildly depending on how you parenthesize. If A is 10×30, B is 30×5, C is 5×60:
- `(AB)C`: 10×30×5 + 10×5×60 = 1500 + 3000 = 4500 ops
- `A(BC)`: 30×5×60 + 10×30×60 = 9000 + 18000 = 27000 ops

The first is 6× cheaper! Matrix Chain Multiplication finds the optimal parenthesization.

**Mental model:** `dp[i][j]` = minimum scalar multiplications to compute the product of matrices `Aᵢ` through `Aⱼ`. Try every possible split point k.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n³) |
| Space Complexity | O(n²) |

---

## ⚙️ How It Works

**Recurrence (try every split point k between i and j):**
```
dp[i][i] = 0                                   (base: single matrix, no multiply)
dp[i][j] = min over k in [i..j-1]:
           dp[i][k] + dp[k+1][j] + p[i-1] * p[k] * p[j]
```

Where `p` is the dimensions array: matrix `Aᵢ` has dimensions `p[i-1] × p[i]`.

1. Fill the table for all chain lengths from 2 to n.
2. For each sub-chain `(i, j)`, try all split points `k`.
3. `dp[1][n]` = answer.

---

## 🔢 Step-by-Step Trace

Matrices: A1(10×30), A2(30×5), A3(5×60) → dimensions array: `p = [10, 30, 5, 60]`

| dp[i][j] | j=1 | j=2          | j=3              |
|----------|-----|--------------|------------------|
| i=1      | 0   | 10×30×5=1500 | min(1500+0+10×5×60=4500, 0+9000+10×30×60=27000) = **4500** |
| i=2      | —   | 0            | 30×5×60=9000     |
| i=3      | —   | —            | 0                |

Answer: `dp[1][3] = 4500` ✅

---

## 🐍 Python Implementation

```python
def matrix_chain_order(p):
    """
    p: dimensions array, where matrix i has dimensions p[i-1] × p[i]
    n matrices total (n = len(p) - 1)
    Returns minimum number of scalar multiplications.
    """
    n = len(p) - 1                           # Number of matrices
    dp = [[0] * (n + 1) for _ in range(n + 1)]
    split = [[0] * (n + 1) for _ in range(n + 1)]  # Track optimal split points

    # Fill for chains of increasing length
    for length in range(2, n + 1):            # Chain length from 2 to n
        for i in range(1, n - length + 2):    # Starting matrix index
            j = i + length - 1               # Ending matrix index
            dp[i][j] = float('inf')

            for k in range(i, j):            # Try every split point
                # Cost = left subchain + right subchain + one multiply step
                cost = dp[i][k] + dp[k + 1][j] + p[i - 1] * p[k] * p[j]
                if cost < dp[i][j]:
                    dp[i][j] = cost
                    split[i][j] = k          # Record best split

    return dp[1][n], split


def print_optimal_parens(split, i, j):
    """Reconstruct the optimal parenthesization."""
    if i == j:
        return f"A{i}"
    k = split[i][j]
    left = print_optimal_parens(split, i, k)
    right = print_optimal_parens(split, k + 1, j)
    return f"({left} × {right})"


# Example: A1(10×30), A2(30×5), A3(5×60)
p = [10, 30, 5, 60]
min_ops, split = matrix_chain_order(p)
print(f"Minimum operations: {min_ops}")      # 4500
print(f"Optimal order: {print_optimal_parens(split, 1, len(p)-1)}")
# (A1 × A2) × A3
```

---

## 🎯 Recognize This Problem When...

- You need to find the **optimal way to split a sequence** into sub-sequences.
- The cost of combining two sub-results depends on their sizes/properties.
- Keywords: "optimal parenthesization", "order of operations", "interval DP".
- Generalizations: optimal BST, polygon triangulation, burst balloons.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Minimizing cost of chained operations | ✅ Direct application |
| Compiler optimizing matrix/tensor ops | ✅ Classic CS application |
| Chain of n items, pair combination cost | ✅ Interval DP pattern |
| Finding actual matrix product value | ❌ This only finds optimal ORDER, not the product |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Rod Cutting](RodCutting.md) | Also "split optimally" but linear, not interval |
| [LCS](LCS.md) | Both use 2D DP tables |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Burst Balloons](https://leetcode.com/problems/burst-balloons/) | LeetCode | 🔴 Hard |
| [Minimum Cost to Cut a Stick](https://leetcode.com/problems/minimum-cost-to-cut-a-stick/) | LeetCode | 🔴 Hard |
| [Strange Printer](https://leetcode.com/problems/strange-printer/) | LeetCode | 🔴 Hard |
