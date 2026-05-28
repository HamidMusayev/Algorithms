### **Knapsack Problem 📈**

Given `n` items with **weight** and **value**, and a knapsack capacity `W`, pick items to **maximize value** without exceeding `W`.

Two variants:
- **0/1 Knapsack** – take or skip each item (no repeats). → DP
- **Fractional Knapsack** – can take fractions of items. → Greedy (see Greedy section)

---

#### **Summary (0/1 Knapsack)**
- **Time Complexity:** O(n × W)
- **Space Complexity:** O(n × W) (can be reduced to O(W))

---

#### **Recurrence**
```
dp[i][w] = dp[i-1][w]                                     if weight[i] > w
         = max(dp[i-1][w], dp[i-1][w-weight[i]] + value[i]) otherwise
```

---

#### **Python Implementation (0/1)**
```python
def knapsack_01(weights, values, W):
    n = len(weights)
    dp = [[0] * (W + 1) for _ in range(n + 1)]

    for i in range(1, n + 1):
        for w in range(W + 1):
            if weights[i - 1] <= w:
                dp[i][w] = max(dp[i - 1][w],
                               dp[i - 1][w - weights[i - 1]] + values[i - 1])
            else:
                dp[i][w] = dp[i - 1][w]
    return dp[n][W]

# Space-optimized (1D array)
def knapsack_01_1d(weights, values, W):
    dp = [0] * (W + 1)
    for i in range(len(weights)):
        for w in range(W, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[W]

# Example
weights = [1, 3, 4, 5]
values  = [1, 4, 5, 7]
print(knapsack_01(weights, values, 7))   # 9 (items 2 + 3)
```

---

#### **When to Use**
✅ Resource allocation, budget problems
✅ Subset selection with constraints
🚫 If items can be split → Fractional Knapsack (greedy)
