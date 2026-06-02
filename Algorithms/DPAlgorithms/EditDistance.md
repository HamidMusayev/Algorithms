# Edit Distance (Levenshtein Distance) ✏️

---

## 🧠 Intuition

Imagine typing one word and needing to transform it into another, one keystroke at a time. Each keystroke is: insert a character, delete a character, or replace a character. Edit distance counts the **minimum keystrokes** needed.

"kitten" → "sitting": k→s (replace), e→i (replace), +g (insert) = **3 edits**.

**Mental model:** `dp[i][j]` = minimum edits to transform `s1[:i]` into `s2[:j]`. Each cell looks at 3 operations from 3 neighbors.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(m × n) |
| Space Complexity | O(m × n) — reducible to O(min(m,n)) |

---

## ⚙️ How It Works

**Recurrence:**
```
dp[i][j] = dp[i-1][j-1]                       if s1[i-1] == s2[j-1]  (no edit needed)
dp[i][j] = 1 + min(
    dp[i-1][j],    # delete from s1
    dp[i][j-1],    # insert into s1
    dp[i-1][j-1]   # replace in s1
)                                              otherwise
```

**Base cases:**
- `dp[i][0] = i` — delete all i characters of s1
- `dp[0][j] = j` — insert all j characters from s2

---

## 🔢 Step-by-Step Trace

s1 = `"cat"`, s2 = `"cut"`

|     | "" | c | u | t |
|-----|----|---|---|---|
| ""  |  0 | 1 | 2 | 3 |
| c   |  1 | 0 | 1 | 2 |
| a   |  2 | 1 | 1 | 2 |
| t   |  3 | 2 | 2 | 1 |

`dp[3][3] = 1` → only 1 edit needed: replace 'a' with 'u' ✅

---

## 🐍 Python Implementation

```python
def edit_distance(s1, s2):
    """Returns the minimum number of insert/delete/replace operations
    to transform s1 into s2."""
    m, n = len(s1), len(s2)
    # dp[i][j] = edit distance between s1[:i] and s2[:j]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    # Base cases: converting to/from empty string
    for i in range(m + 1):
        dp[i][0] = i    # delete i characters from s1
    for j in range(n + 1):
        dp[0][j] = j    # insert j characters

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1]          # Characters match — no edit needed
            else:
                dp[i][j] = 1 + min(
                    dp[i - 1][j],      # Delete: remove s1[i-1]
                    dp[i][j - 1],      # Insert: add s2[j-1] to s1
                    dp[i - 1][j - 1]   # Replace: change s1[i-1] to s2[j-1]
                )

    return dp[m][n]


def edit_distance_space_optimized(s1, s2):
    """O(min(m,n)) space using only two rows."""
    m, n = len(s1), len(s2)
    if m < n:
        s1, s2 = s2, s1
        m, n = n, m    # Ensure m >= n (use shorter string for columns)

    prev = list(range(n + 1))    # Previous row
    for i in range(1, m + 1):
        curr = [i] + [0] * n    # Current row: base case curr[0] = i
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                curr[j] = prev[j - 1]
            else:
                curr[j] = 1 + min(prev[j], curr[j - 1], prev[j - 1])
        prev = curr

    return prev[n]


# Examples
print(edit_distance("kitten", "sitting"))    # 3
print(edit_distance("cat", "cut"))           # 1
print(edit_distance("", "abc"))              # 3
print(edit_distance("abc", "abc"))           # 0
```

---

## 🎯 Recognize This Problem When...

- You need to measure **similarity between two strings** by counting minimum changes.
- Keywords: "minimum operations to convert", "spell checker", "fuzzy matching", "diff".
- The allowed operations are insert, delete, replace (or some subset).
- You see strings that are similar but not identical and need to quantify the difference.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Spell checking / autocorrect | ✅ Classic application |
| DNA mutation distance | ✅ Biology sequence alignment |
| Git diff / document comparison | ✅ With LCS variant |
| Only insertions/deletions (no replace) | ✅ Modify to remove the replace option |
| Very long strings (millions of chars) | ❌ O(mn) too slow; use approximate methods |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [LCS](LCS.md) | Edit distance = m + n - 2 × LCS length (for only insert/delete) |
| [Knapsack](Knapsack.md) | Same 2D DP table structure |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Edit Distance](https://leetcode.com/problems/edit-distance/) | LeetCode | 🔴 Hard |
| [Minimum ASCII Delete Sum](https://leetcode.com/problems/minimum-ascii-delete-sum-for-two-strings/) | LeetCode | 🟡 Medium |
| [One Edit Distance](https://leetcode.com/problems/one-edit-distance/) | LeetCode | 🟡 Medium |
| [Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/) | LeetCode | 🟡 Medium |
