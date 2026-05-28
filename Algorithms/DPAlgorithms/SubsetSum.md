### **Subset Sum Problem 📈**

Given a set of positive integers and a target sum, determine whether **any subset** sums to the target.

Classic DP problem; closely related to 0/1 Knapsack.

---

#### **Summary**
- **Time Complexity:** O(n × target)
- **Space Complexity:** O(n × target) (reducible to O(target))

> NP-hard in the general (exponential) sense; pseudo-polynomial via DP.

---

#### **Recurrence**
```
dp[i][s] = dp[i-1][s]                               if nums[i-1] > s
         = dp[i-1][s] or dp[i-1][s - nums[i-1]]     otherwise
```

---

#### **Python Implementation**
```python
def subset_sum(nums, target):
    n = len(nums)
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        dp[i][0] = True

    for i in range(1, n + 1):
        for s in range(1, target + 1):
            if nums[i - 1] > s:
                dp[i][s] = dp[i - 1][s]
            else:
                dp[i][s] = dp[i - 1][s] or dp[i - 1][s - nums[i - 1]]
    return dp[n][target]

# Space-optimized
def subset_sum_1d(nums, target):
    dp = [False] * (target + 1)
    dp[0] = True
    for num in nums:
        for s in range(target, num - 1, -1):
            dp[s] = dp[s] or dp[s - num]
    return dp[target]

# Example
print(subset_sum([3, 34, 4, 12, 5, 2], 9))   # True (4+5 or 3+4+2)
```

---

#### **When to Use**
✅ Partition problems, equal-sum splits
✅ Foundation for Knapsack-family DPs
✅ Capacity / budget feasibility
