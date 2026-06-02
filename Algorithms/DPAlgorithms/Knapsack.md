# 0/1 Knapsack Problem 🎒

---

## 🧠 Intuition

You're a thief with a backpack that can hold at most `W` kg. You see `n` items, each with a weight and value. You can take each item or leave it (no splitting). You want to maximize total value without exceeding the weight limit.

The key insight: for each item, you have exactly two choices — **take it** or **leave it**. The optimal decision for item `i` with remaining capacity `w` depends only on the optimal solution for previous items with either `w` or `w - weight[i]` capacity.

**Mental model:** Fill a 2D table where `dp[i][w]` = "max value using first i items with capacity w". Each cell either skips item i or takes it.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n × W) |
| Space Complexity | O(n × W) — reducible to O(W) |

> This is **pseudo-polynomial** — polynomial in W, but W can be large. NP-hard in the general sense.

---

## ⚙️ How It Works

**Recurrence:**
```
dp[i][w] = dp[i-1][w]                               if weight[i] > w  (can't take item i)
dp[i][w] = max(dp[i-1][w],                          otherwise
               dp[i-1][w - weight[i]] + value[i])
```

1. Initialize `dp[0][w] = 0` for all w (no items = no value).
2. For each item `i` from 1 to n:
   - For each capacity `w` from 0 to W:
     - If item doesn't fit (`weight[i] > w`): copy from above `dp[i-1][w]`.
     - If item fits: take max of skipping vs taking.
3. Answer is `dp[n][W]`.

---

## 🔢 Step-by-Step Trace

Items: weights=[1,3,4,5], values=[1,4,5,7], W=7

| i\w | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|-----|---|---|---|---|---|---|---|---|
| 0   | 0 | 0 | 0 | 0 | 0 | 0 | 0 | 0 |
| 1(w=1,v=1)| 0|1|1|1|1|1|1|1|
| 2(w=3,v=4)| 0|1|1|4|5|5|5|5|
| 3(w=4,v=5)| 0|1|1|4|5|6|6|9|
| 4(w=5,v=7)| 0|1|1|4|5|7|8|9|

Answer: `dp[4][7] = 9` (take items 2+3: 3+4=7kg, 4+5=9 value) ✅

---

## 🐍 Python Implementation

```python
def knapsack_01(weights, values, W):
    """2D DP table — clear and easy to understand."""
    n = len(weights)
    dp = [[0] * (W + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(W + 1):
            # Option 1: don't take item i
            dp[i][w] = dp[i - 1][w]
            # Option 2: take item i (only if it fits)
            if weights[i - 1] <= w:
                take = dp[i - 1][w - weights[i - 1]] + values[i - 1]
                dp[i][w] = max(dp[i][w], take)

    return dp[n][W]


def knapsack_01_optimized(weights, values, W):
    """Space-optimized: O(W) — traverse weights backwards to avoid using item twice."""
    dp = [0] * (W + 1)
    for i in range(len(weights)):
        # Traverse backwards: ensures each item is only used once
        for w in range(W, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[W]


# Example
weights = [1, 3, 4, 5]
values  = [1, 4, 5, 7]
print(knapsack_01(weights, values, 7))           # 9
print(knapsack_01_optimized(weights, values, 7)) # 9
```

---

## 🎯 Recognize This Problem When...

- You must **select a subset** of items to maximize value/profit subject to a **capacity constraint**.
- Each item can be taken **at most once** (0 or 1 times).
- Keywords: "choose items without exceeding budget/weight", "yes/no for each item".
- The decision for each item is binary — this is the "0/1" in the name.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Subset selection with binary choices       | ✅ Classic 0/1 Knapsack                       |
| Resource allocation, portfolio selection   | ✅ Direct application                         |
| Items can be split into fractions          | ❌ Use Fractional Knapsack (greedy)           |
| Items can be used multiple times           | ❌ Use Unbounded Knapsack variant             |
| W is astronomically large                  | ❌ DP is too slow; use branch-and-bound       |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Subset Sum](SubsetSum.md) | Special case: values=weights, target sum instead of max value |
| [Fractional Knapsack](../GreedyAlgorithms/FractionalKnapsack.md) | Items splittable → greedy works |
| [Coin Change](CoinChange.md) | Unbounded knapsack variant (unlimited quantity per item) |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) | LeetCode | 🟡 Medium |
| [Target Sum](https://leetcode.com/problems/target-sum/) | LeetCode | 🟡 Medium |
| [Last Stone Weight II](https://leetcode.com/problems/last-stone-weight-ii/) | LeetCode | 🟡 Medium |
| [Knapsack](https://www.hackerrank.com/challenges/unbounded-knapsack/problem) | HackerRank | 🟡 Medium |
