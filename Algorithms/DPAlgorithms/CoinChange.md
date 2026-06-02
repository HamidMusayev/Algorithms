# Coin Change Problem 💰

---

## 🧠 Intuition

You have a cash register with coins of certain denominations. A customer gives you an amount to make change. The Coin Change problem asks: what's the **minimum number of coins** to make that amount? Or: in **how many ways** can you make it?

The key DP insight: if you know the minimum coins for all amounts from 1 to `amount-1`, you can compute it for `amount` by trying each coin and taking the best option.

**Mental model:** `dp[a]` = minimum coins needed to make amount `a`. Build it bottom-up from 0 to `amount`.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n × amount) — n = number of coin types |
| Space Complexity | O(amount) |

---

## ⚙️ How It Works

**Variant 1 — Minimum coins:**
```
dp[0] = 0
dp[a] = min(dp[a - coin] + 1) for each coin ≤ a
```

**Variant 2 — Number of ways:**
```
dp[0] = 1
dp[a] += dp[a - coin] for each coin ≤ a
```
> ⚠️ Loop order matters: **coin outer, amount inner** → counts unordered combinations. **Amount outer, coin inner** → counts ordered permutations.

---

## 🔢 Step-by-Step Trace

Coins = `[1, 2, 5]`, amount = 6

**Minimum coins:**

| a | Try coin 1 | Try coin 2 | Try coin 5 | dp[a] |
|---|------------|------------|------------|-------|
| 0 | —          | —          | —          | 0     |
| 1 | dp[0]+1=1  | —          | —          | 1     |
| 2 | dp[1]+1=2  | dp[0]+1=1  | —          | 1     |
| 3 | dp[2]+1=2  | dp[1]+1=2  | —          | 2     |
| 4 | dp[3]+1=3  | dp[2]+1=2  | —          | 2     |
| 5 | dp[4]+1=3  | dp[3]+1=3  | dp[0]+1=1  | 1     |
| 6 | dp[5]+1=2  | dp[4]+1=3  | dp[1]+1=2  | 2     |

Answer: 2 coins (5+1) ✅

---

## 🐍 Python Implementation

```python
def coin_change_min(coins, amount):
    """Find the minimum number of coins to make 'amount'. Returns -1 if impossible."""
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0                           # Base case: 0 coins for amount 0

    for a in range(1, amount + 1):
        for coin in coins:
            if coin <= a:               # Can we use this coin?
                dp[a] = min(dp[a], dp[a - coin] + 1)   # +1 for using this coin

    return dp[amount] if dp[amount] != float('inf') else -1


def coin_change_ways(coins, amount):
    """Count the number of COMBINATIONS (unordered) to make 'amount'."""
    dp = [0] * (amount + 1)
    dp[0] = 1                           # Base case: 1 way to make 0 (use no coins)

    for coin in coins:                  # Outer loop = coin → counts combinations
        for a in range(coin, amount + 1):
            dp[a] += dp[a - coin]       # Add number of ways to make (a - coin)

    return dp[amount]


def coin_change_permutations(coins, amount):
    """Count the number of PERMUTATIONS (ordered) to make 'amount'."""
    dp = [0] * (amount + 1)
    dp[0] = 1

    for a in range(1, amount + 1):      # Outer loop = amount → counts permutations
        for coin in coins:
            if coin <= a:
                dp[a] += dp[a - coin]

    return dp[amount]


# Examples
print(coin_change_min([1, 2, 5], 11))       # 3  (5+5+1)
print(coin_change_ways([1, 2, 5], 5))        # 4  ({5}, {2,2,1}, {2,1,1,1}, {1,1,1,1,1})
print(coin_change_permutations([1, 2], 3))   # 3  (1+1+1, 1+2, 2+1)
```

---

## 🎯 Recognize This Problem When...

- You must make a **target amount** using items of given "sizes" (coins) with **unlimited supply**.
- Keywords: "ways to make change", "minimum coins", "reach target amount".
- The items can be used any number of times (unbounded knapsack variant).
- Classic signal: "you have coins of denominations [x, y, z], make amount A".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Minimum coins/items to reach a target | ✅ Direct application |
| Count ways to reach a target (combinations) | ✅ Use coin-outer loop order |
| Count ordered arrangements (permutations) | ✅ Use amount-outer loop order |
| Each coin can only be used once | ❌ Use 0/1 Knapsack instead |
| Canonical coin systems (US cents) | ❌ Greedy works — always use largest coin first |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Knapsack](Knapsack.md) | 0/1 Knapsack = each item once; Coin Change = unbounded |
| [Subset Sum](SubsetSum.md) | Special case: can you make amount exactly? |
| [Fibonacci](Fibonacci.md) | Climbing stairs = coin change with coins [1,2] |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Coin Change](https://leetcode.com/problems/coin-change/) | LeetCode | 🟡 Medium |
| [Coin Change II](https://leetcode.com/problems/coin-change-ii/) | LeetCode | 🟡 Medium |
| [Combination Sum IV](https://leetcode.com/problems/combination-sum-iv/) | LeetCode | 🟡 Medium |
| [Perfect Squares](https://leetcode.com/problems/perfect-squares/) | LeetCode | 🟡 Medium |
