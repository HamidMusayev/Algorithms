### **Edit Distance (Levenshtein Distance) 📈**

Minimum number of single-character edits (**insert**, **delete**, **replace**) to transform string `a` into string `b`.

Example: `kitten → sitting` = **3** (k→s, e→i, +g).

---

#### **Summary**
- **Time Complexity:** O(m × n)
- **Space Complexity:** O(m × n) (reducible to O(min(m,n)))

---

#### **Recurrence**
```
dp[i][j] = dp[i-1][j-1]                                   if a[i-1] == b[j-1]
         = 1 + min(dp[i-1][j],     # delete
                   dp[i][j-1],     # insert
                   dp[i-1][j-1])   # replace
```

---

#### **Python Implementation**
```python
def edit_distance(a, b):
    m, n = len(a), len(b)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(m + 1): dp[i][0] = i
    for j in range(n + 1): dp[0][j] = j

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]
            else:
                dp[i][j] = 1 + min(dp[i - 1][j],
                                   dp[i][j - 1],
                                   dp[i - 1][j - 1])
    return dp[m][n]

# Example
print(edit_distance("kitten", "sitting"))   # 3
```

---

#### **When to Use**
✅ Spell checkers, fuzzy matching
✅ DNA mutation distance
✅ Diff / merge tools
