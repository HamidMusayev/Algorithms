# Longest Increasing Subsequence (LIS) 📈

---

## 🧠 Intuition

Imagine patience sorting — a card game where you place cards on piles, always placing a card on the leftmost pile whose top card is ≥ your card (or making a new pile). The number of piles at the end equals the LIS length.

More intuitively: think of an array as a terrain. The LIS is the longest "staircase" you can climb — strictly going up — picking elements from left to right without backtracking.

**Mental model:** `dp[i]` = length of the longest increasing subsequence ending exactly at index `i`.

---

## 📊 Complexity

| Approach | Time | Space |
|----------|------|-------|
| DP (O(n²)) | O(n²) | O(n) |
| Binary search (patience sorting) | O(n log n) | O(n) |

---

## ⚙️ How It Works

**O(n²) DP approach:**
```
dp[i] = max(dp[j] + 1) for all j < i where arr[j] < arr[i]
```
Base: `dp[i] = 1` (each element alone is an LIS of length 1).

**O(n log n) binary search approach (patience sort):**
1. Maintain a `tails` array: `tails[i]` = smallest tail element for all increasing subsequences of length `i+1`.
2. For each element, binary search in `tails` for its insertion point:
   - If larger than all tails → append (new longer LIS found).
   - Otherwise → replace the found position (maintain smallest possible tail).

---

## 🔢 Step-by-Step Trace

Array: `[10, 9, 2, 5, 3, 7, 101, 18]`

**O(n²) DP:**

| i | arr[i] | dp[i] (LIS ending here) | Explanation |
|---|--------|--------------------------|-------------|
| 0 | 10     | 1                        | Just [10]   |
| 1 | 9      | 1                        | Just [9]    |
| 2 | 2      | 1                        | Just [2]    |
| 3 | 5      | 2                        | [2,5]       |
| 4 | 3      | 2                        | [2,3]       |
| 5 | 7      | 3                        | [2,3,7] or [2,5,7] |
| 6 | 101    | 4                        | [2,3,7,101] |
| 7 | 18     | 4                        | [2,3,7,18]  |

`max(dp) = 4` ✅

---

## 🐍 Python Implementation

```python
def lis_n2(arr):
    """O(n²) DP — easier to understand, reconstructible."""
    if not arr:
        return 0
    n = len(arr)
    dp = [1] * n    # Every element alone is an LIS of length 1

    for i in range(1, n):
        for j in range(i):
            if arr[j] < arr[i]:             # arr[j] can extend the subsequence ending at i
                dp[i] = max(dp[i], dp[j] + 1)

    return max(dp)


from bisect import bisect_left

def lis_nlogn(arr):
    """O(n log n) — patience sorting via binary search."""
    tails = []   # tails[i] = smallest tail of all IS of length i+1

    for x in arr:
        pos = bisect_left(tails, x)   # Find leftmost position where tails[pos] >= x
        if pos == len(tails):
            tails.append(x)           # x extends the longest IS found so far
        else:
            tails[pos] = x            # Replace with smaller tail (better for future)

    return len(tails)    # Number of piles = LIS length
    # Note: tails itself is NOT the LIS — only its length is correct


# Example
arr = [10, 9, 2, 5, 3, 7, 101, 18]
print("O(n²):", lis_n2(arr))       # 4
print("O(n log n):", lis_nlogn(arr))  # 4
```

---

## 🎯 Recognize This Problem When...

- You need the **longest strictly increasing** subsequence (not substring) of an array.
- Keywords: "longest increasing", "longest chain", "box stacking", "longest rising".
- The problem can be reduced to: find the longest chain of elements satisfying a pairwise constraint.
- Patience sorting hints: "pile" problems, "stack boxes", "schedule without overlap by some measure".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Longest strictly increasing subsequence | ✅ Direct application |
| Box stacking (each box's dims < next) | ✅ Sort by one dim, LIS on another |
| Longest chain of word pairs | ✅ LIS variant |
| Need the actual subsequence (not just length) | ✅ Use O(n²) DP with predecessor tracking |
| Longest non-decreasing subsequence | ✅ Change `<` to `<=` in binary search |
| Contiguous increasing subarray | ❌ Use a simple linear scan instead |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [LCS](LCS.md) | LCS of arr and sorted(arr) = LIS length |
| [Knapsack](Knapsack.md) | Similar "extend or skip" DP pattern |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Longest Increasing Subsequence](https://leetcode.com/problems/longest-increasing-subsequence/) | LeetCode | 🟡 Medium |
| [Russian Doll Envelopes](https://leetcode.com/problems/russian-doll-envelopes/) | LeetCode | 🔴 Hard |
| [Number of LIS](https://leetcode.com/problems/number-of-longest-increasing-subsequence/) | LeetCode | 🟡 Medium |
| [Longest Arithmetic Subsequence](https://leetcode.com/problems/longest-arithmetic-subsequence/) | LeetCode | 🟡 Medium |
