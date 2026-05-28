### **Coin Change Problem 📈**

Given coin denominations and a target amount, two common variants:

1. **Min coins** to make the amount (or `-1` if impossible).
2. **Number of ways** to make the amount.

---

#### **Summary**
- **Time Complexity:** O(n × amount)
- **Space Complexity:** O(amount)

---

#### **Python Implementation**

**1. Minimum number of coins**
```python
def coin_change_min(coins, amount):
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for a in range(1, amount + 1):
        for c in coins:
            if c <= a:
                dp[a] = min(dp[a], dp[a - c] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1

print(coin_change_min([1, 2, 5], 11))   # 3  (5+5+1)
```

**2. Number of ways to make the amount**
```python
def coin_change_ways(coins, amount):
    dp = [0] * (amount + 1)
    dp[0] = 1
    for c in coins:                # outer loop over coins → unordered combinations
        for a in range(c, amount + 1):
            dp[a] += dp[a - c]
    return dp[amount]

print(coin_change_ways([1, 2, 5], 5))   # 4
```

> ⚠️ Loop order matters: coin-outer counts **combinations**; amount-outer counts **permutations**.

---

#### **When to Use**
✅ Currency / making change problems
✅ Unbounded subset-sum variants
🚫 If coins are very large or amount unbounded, consider greedy (only valid for canonical systems)
