### **Rod Cutting Problem 📈**

Given a rod of length `n` and a price table `price[i]` for each cut length `i`, find the **maximum revenue** obtainable by cutting the rod into pieces and selling them.

---

#### **Summary**
- **Time Complexity:** O(n²)
- **Space Complexity:** O(n)

---

#### **Recurrence**
```
dp[n] = max over i in [1..n]: price[i] + dp[n - i]
```

This is essentially **unbounded knapsack** where weights = lengths and values = prices.

---

#### **Python Implementation**
```python
def rod_cutting(price, n):
    # price[i] = price for piece of length i+1
    dp = [0] * (n + 1)
    for length in range(1, n + 1):
        best = 0
        for i in range(1, length + 1):
            best = max(best, price[i - 1] + dp[length - i])
        dp[length] = best
    return dp[n]

# Example
prices = [1, 5, 8, 9, 10, 17, 17, 20]   # length 1..8
print(rod_cutting(prices, 8))           # 22  (cut into 2 + 6)
```

---

#### **When to Use**
✅ Pricing & inventory splitting problems
✅ Classic interval DP teaching example
✅ Unbounded knapsack pattern
