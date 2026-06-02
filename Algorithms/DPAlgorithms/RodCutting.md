# Rod Cutting Problem ✂️

---

## 🧠 Intuition

You have a steel rod of length `n` and a price list for each cut length. You want to cut the rod into pieces to maximize total revenue. The key insight: you can cut it multiple ways and the same length can be used multiple times (unlimited cuts of each size).

This is essentially an **unbounded knapsack** — the rod's length is the "capacity", each cut length is an "item weight", and its price is the "item value".

**Mental model:** `dp[i]` = maximum revenue from a rod of length `i`. For each length, try all possible first cuts.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n²) |
| Space Complexity | O(n) |

---

## ⚙️ How It Works

**Recurrence:**
```
dp[0] = 0
dp[length] = max over i in [1..length]:
             price[i] + dp[length - i]
```

For each rod length from 1 to n:
- Try every possible first cut of size `i` (1 ≤ i ≤ length).
- Revenue = `price[i]` (for piece of length i) + `dp[length - i]` (optimal for remainder).
- Take the maximum.

---

## 🔢 Step-by-Step Trace

Prices: `[1, 5, 8, 9, 10, 17, 17, 20]` (prices[i] = price for length i+1)

| length | Try cuts | Best option | dp[length] |
|--------|----------|-------------|-----------|
| 1      | cut=1: 1+dp[0]=1 | — | 1 |
| 2      | cut=1: 1+1=2, cut=2: 5+0=5 | cut=2 | 5 |
| 3      | cut=1: 1+5=6, cut=2: 5+1=6, cut=3: 8+0=8 | cut=3 | 8 |
| 4      | cut=1: 1+8=9, cut=2: 5+5=10, cut=4: 9 | cut=2+2 | 10 |
| 8      | ... | 2+6 or all 2s → 20 | 22 |

`dp[8] = 22` (cut into length 2 + length 6: 5 + 17 = 22) ✅

---

## 🐍 Python Implementation

```python
def rod_cutting(prices, n):
    """
    prices: list where prices[i] = selling price for rod of length (i+1)
    n: total rod length
    Returns maximum revenue.
    """
    dp = [0] * (n + 1)   # dp[0] = 0 (no rod, no revenue)

    for length in range(1, n + 1):
        max_val = 0
        for cut in range(1, length + 1):   # Try every possible first cut size
            # price for piece of length 'cut' + best revenue for remaining rod
            max_val = max(max_val, prices[cut - 1] + dp[length - cut])
        dp[length] = max_val

    return dp[n]


def rod_cutting_with_cuts(prices, n):
    """Returns (max_revenue, list_of_cuts) by tracking which cuts were made."""
    dp = [0] * (n + 1)
    cuts = [0] * (n + 1)   # cuts[i] = optimal first cut for rod of length i

    for length in range(1, n + 1):
        for cut in range(1, length + 1):
            val = prices[cut - 1] + dp[length - cut]
            if val > dp[length]:
                dp[length] = val
                cuts[length] = cut    # Record best first cut

    # Reconstruct the cut sequence
    result = []
    remaining = n
    while remaining > 0:
        result.append(cuts[remaining])
        remaining -= cuts[remaining]

    return dp[n], result


# Example
prices = [1, 5, 8, 9, 10, 17, 17, 20]   # Length 1=1$, 2=5$, ..., 8=20$
print(rod_cutting(prices, 8))             # 22

revenue, cuts = rod_cutting_with_cuts(prices, 8)
print(f"Revenue: {revenue}, Cuts: {cuts}")
# Revenue: 22, Cuts: [2, 6] (or [6, 2])
```

---

## 🎯 Recognize This Problem When...

- You need to **divide a resource** into pieces to maximize total value.
- Each piece size can be used **multiple times** (unbounded).
- Keywords: "cut rod/pipe/board to maximize profit", "divide optimally".
- The problem has the structure: "pick any combination of sizes (with repetition) summing to N, maximize total value".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Maximize value by cutting into pieces (repeatable sizes) | ✅ Directly applicable |
| Pricing optimization with divisible products | ✅ Good model |
| Each piece size can only be used once | ❌ Use 0/1 Knapsack instead |
| Minimize cost instead of maximize | ✅ Flip to minimization — same structure |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Knapsack](Knapsack.md) | Rod Cutting = unbounded knapsack (items reusable) |
| [Coin Change](CoinChange.md) | Same pattern — pick items (coins/cuts) to reach a sum |
| [Matrix Chain Multiplication](MatrixChainMultiplication.md) | Also interval DP — "how to split optimally" |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Integer Break](https://leetcode.com/problems/integer-break/) | LeetCode | 🟡 Medium |
| [Coin Change](https://leetcode.com/problems/coin-change/) | LeetCode | 🟡 Medium |
| [Rod Cutting](https://www.geeksforgeeks.org/cutting-a-rod-dp-13/) | GeeksForGeeks | 🟡 Medium |
