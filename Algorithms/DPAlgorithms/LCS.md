# Longest Common Subsequence (LCS) 📝

---

## 🧠 Intuition

Imagine two DNA strands: `ABCBDAB` and `BDCAB`. Which characters appear in the same relative order in both? The LCS is the longest sequence you can "draw lines" between in the two strings without crossing any lines.

Unlike substring (must be contiguous), a **subsequence** can skip characters — it just has to maintain order. The overlapping subproblem: "LCS of s1[0..i] and s2[0..j]" only depends on "LCS of shorter prefixes."

**Mental model:** Build a 2D table where `dp[i][j]` = length of LCS of first i chars of s1 and first j chars of s2.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(m × n) |
| Space Complexity | O(m × n) — reducible to O(min(m, n)) |

> m = length of string 1, n = length of string 2.

---

## ⚙️ How It Works

**Recurrence:**
```
dp[i][j] = dp[i-1][j-1] + 1            if s1[i-1] == s2[j-1]  (characters match)
dp[i][j] = max(dp[i-1][j], dp[i][j-1]) otherwise               (take best without one char)
```

1. Create `dp` table of size `(m+1) × (n+1)`, initialized to 0.
2. For each character pair `(s1[i-1], s2[j-1])`:
   - Match: extend the diagonal by 1.
   - No match: take max of skipping one character from either string.
3. `dp[m][n]` = LCS length.
4. Trace back diagonally to reconstruct the actual LCS string.

---

## 🔢 Step-by-Step Trace

s1 = `"ABC"`, s2 = `"AC"`

|     | "" | A | C |
|-----|----|---|---|
| ""  |  0 | 0 | 0 |
| A   |  0 | 1 | 1 |
| B   |  0 | 1 | 1 |
| C   |  0 | 1 | 2 |

`dp[3][2] = 2` → LCS = `"AC"` ✅

Full example: LCS of `"ABCBDAB"` and `"BDCAB"` = `"BCAB"` (length 4)

---

## 🐍 Python Implementation

```python
def lcs_length(s1, s2):
    """Returns length of the Longest Common Subsequence."""
    m, n = len(s1), len(s2)
    # dp[i][j] = LCS length of s1[:i] and s2[:j]
    dp = [[0] * (n + 1) for _ in range(m + 1)]

    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:           # Characters match — extend diagonal
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:                                  # No match — take best of skipping one
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    return dp[m][n]


def lcs_string(s1, s2):
    """Returns the actual LCS string by tracing back through the DP table."""
    m, n = len(s1), len(s2)
    dp = [[0] * (n + 1) for _ in range(m + 1)]
    for i in range(1, m + 1):
        for j in range(1, n + 1):
            if s1[i - 1] == s2[j - 1]:
                dp[i][j] = dp[i - 1][j - 1] + 1
            else:
                dp[i][j] = max(dp[i - 1][j], dp[i][j - 1])

    # Trace back to reconstruct the LCS
    i, j = m, n
    result = []
    while i > 0 and j > 0:
        if s1[i - 1] == s2[j - 1]:    # This character is in the LCS
            result.append(s1[i - 1])
            i -= 1; j -= 1
        elif dp[i - 1][j] >= dp[i][j - 1]:
            i -= 1                      # Move up (skip s1 character)
        else:
            j -= 1                      # Move left (skip s2 character)

    return ''.join(reversed(result))


# Example
print(lcs_length("ABCBDAB", "BDCAB"))   # 4
print(lcs_string("ABCBDAB", "BDCAB"))   # "BCAB"
```

---

## 🎯 Recognize This Problem When...

- You need to find the **longest common part** of two sequences (maintaining order but not contiguous).
- Keywords: "common subsequence", "sequence alignment", "minimum edits between sequences".
- The problem involves **DNA/protein sequence alignment** (bioinformatics).
- You need to find how **similar** two strings/sequences are.
- Diff utilities (git diff) use LCS to find changed lines.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Comparing two sequences for common patterns | ✅ Core use case |
| DNA/protein sequence alignment | ✅ Classic bioinformatics application |
| Building diff tools (changed lines) | ✅ Standard approach |
| Need contiguous matching (substring) | ❌ Use KMP or Z-Algorithm instead |
| Very long sequences (millions of chars) | ❌ O(mn) becomes too slow; use approximate methods |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Edit Distance](EditDistance.md) | Also 2D DP on two strings; LCS → edit distance transformation |
| [LIS](LIS.md) | LCS of array and its sorted version = LIS |
| [Knapsack](Knapsack.md) | Same 2D DP table structure |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Longest Common Subsequence](https://leetcode.com/problems/longest-common-subsequence/) | LeetCode | 🟡 Medium |
| [Shortest Common Supersequence](https://leetcode.com/problems/shortest-common-supersequence/) | LeetCode | 🔴 Hard |
| [Delete Operation for Two Strings](https://leetcode.com/problems/delete-operation-for-two-strings/) | LeetCode | 🟡 Medium |
| [Longest Palindromic Subsequence](https://leetcode.com/problems/longest-palindromic-subsequence/) | LeetCode | 🟡 Medium |
