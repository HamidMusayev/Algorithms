### **Longest Common Subsequence (LCS) 📈**

Given two strings, find the length (or the actual sequence) of the **longest subsequence** present in both. A subsequence keeps order but doesn't have to be contiguous.

Example: LCS of `"ABCBDAB"` and `"BDCAB"` is `"BCAB"` (length 4).

---

#### **Summary**
- **Time Complexity:** O(m × n)
- **Space Complexity:** O(m × n) (reducible to O(min(m,n)))

---

#### **Recurrence**
```
dp[i][j] = dp[i-1][j-1] + 1                  if a[i-1] == b[j-1]
         = max(dp[i-1][j], dp[i][j-1])       otherwise
```

---

#### **Python Implementation**
```python
def lcs_length(a, b):
    m, n = len(a), len(b)
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])
    return dp[m][n]

def lcs_string(a, b):
    m, n = len(a), len(b)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if a[i - 1] == b[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    # Reconstruct LCS
    i, j, result = m, n, []
    while i > 0 and j > 0:
        if a[i - 1] == b[j - 1]:
            result.append(a[i - 1])
            i -= 1; j -= 1
        elif dp[i - 1][j] >= dp[i][j - 1]:
            i -= 1
        else:
            j -= 1
    return ''.join(reversed(result))

# Example
print(lcs_length("ABCBDAB", "BDCAB"))   # 4
print(lcs_string("ABCBDAB", "BDCAB"))   # "BCAB"
```

---

#### **When to Use**
✅ DNA / protein sequence alignment
✅ Diff utilities, version control
✅ Spell correction (related to edit distance)
