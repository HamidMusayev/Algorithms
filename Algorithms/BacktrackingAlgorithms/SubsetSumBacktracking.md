# Subset Sum (Backtracking) 🔍

---

## 🧠 Intuition

While the DP version of Subset Sum only answers "is it possible?", backtracking **finds and enumerates all subsets** that hit the target sum. The approach: at each step, decide to **include** or **exclude** the current element, recurse, and **undo** the decision before trying the other option.

**Mental model:** "For each element, branch into two choices: take it or leave it. Prune branches where the running sum exceeds the target (with positive numbers)."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(2ⁿ) worst case (explores all subsets) |
| Space Complexity | O(n) recursion depth |
| With pruning | Much better in practice for positive numbers |

---

## ⚙️ How It Works

1. Sort the input (enables pruning).
2. **Choose:** consider including `nums[i]` in the current path.
3. **Constrain:** if `total + nums[i] > target`, stop exploring this branch.
4. **Goal:** if `total == target`, record the current path as a valid subset.
5. **Recurse:** call backtrack with `i+1`.
6. **Undo:** pop `nums[i]` from the path (backtrack).

---

## 🔢 Step-by-Step Trace

nums = `[2, 3, 5, 6]`, target = `8`

```
backtrack(start=0, path=[], total=0)
├─ Include 2: backtrack(1, [2], 2)
│  ├─ Include 3: backtrack(2, [2,3], 5)
│  │  ├─ Include 5: backtrack(3, [2,3,5], 10) → 10 > 8, PRUNE ✗
│  │  └─ Include 6: backtrack(4, [2,3,6], 11) → 11 > 8, PRUNE ✗
│  │  → No solution with [2,3]
│  ├─ Include 5: backtrack(3, [2,5], 7)
│  │  ├─ Include 6: backtrack(4, [2,5,6], 13) → PRUNE ✗
│  │  → No solution with [2,5]
│  └─ Include 6: backtrack(4, [2,6], 8) → 8 == 8, FOUND! ✅ → [[2,6]]
├─ Include 3: backtrack(1, [3], 3)
│  └─ Include 5: backtrack(2, [3,5], 8) → 8 == 8, FOUND! ✅ → [[3,5]]
├─ Include 5: backtrack(2, [5], 5)
│  └─ Include 6: backtrack(3, [5,6], 11) → PRUNE ✗
└─ Include 6: backtrack(3, [6], 6)
   → 6 < 8 but no elements ≥ 2 remain to complete it
```

Result: `[[2, 6], [3, 5]]` ✅

---

## 🐍 Python Implementation

```python
def subset_sum_all(nums, target):
    """
    Find ALL subsets of nums that sum to target.
    Returns list of valid subsets.
    """
    results = []
    nums.sort()    # Sort enables pruning (stop when sum exceeds target)

    def backtrack(start, path, total):
        if total == target:
            results.append(list(path))   # Found a valid subset — record it
            return                        # No need to go deeper (all nums > 0)

        for i in range(start, len(nums)):
            if total + nums[i] > target:
                break               # Pruning: since sorted, all remaining are >= nums[i]

            path.append(nums[i])                         # Choose nums[i]
            backtrack(i + 1, path, total + nums[i])      # Explore
            path.pop()                                   # Undo choice (backtrack)

    backtrack(0, [], 0)
    return results


def subset_sum_exists(nums, target):
    """Check if ANY subset sums to target (returns True/False with early exit)."""
    nums.sort()

    def backtrack(start, total):
        if total == target:
            return True
        for i in range(start, len(nums)):
            if total + nums[i] > target:
                break
            if backtrack(i + 1, total + nums[i]):
                return True
        return False

    return backtrack(0, 0)


# Examples
print(subset_sum_all([2, 3, 5, 6, 8, 10], 10))
# [[2, 3, 5], [2, 8], [10]]

print(subset_sum_all([2, 3, 6, 7], 7))
# [[7]]

print(subset_sum_exists([3, 34, 4, 12, 5, 2], 9))
# True
```

---

## 🎯 Recognize This Problem When...

- You need to **enumerate all subsets** with a specific property (not just check feasibility).
- Keywords: "find all combinations", "list all subsets summing to target".
- You need the actual subsets, not just a yes/no answer.
- The input is small (n ≤ 25) so O(2ⁿ) is acceptable.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Need all valid subsets enumerated | ✅ Only way to get all solutions |
| Small n (≤ 25) with good pruning | ✅ Fast in practice |
| Only need yes/no (feasibility) | ❌ Use DP (SubsetSum) — much faster |
| Large n with large target | ❌ Exponential — use DP or meet-in-the-middle |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Subset Sum DP](../DPAlgorithms/SubsetSum.md) | Feasibility only, O(n×target) |
| [Combination Sum](NQueens.md) | Same pattern but items reusable |
| [N-Queens](NQueens.md) | Same try→check→undo backtracking structure |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Combination Sum](https://leetcode.com/problems/combination-sum/) | LeetCode | 🟡 Medium |
| [Combination Sum II](https://leetcode.com/problems/combination-sum-ii/) | LeetCode | 🟡 Medium |
| [Subsets](https://leetcode.com/problems/subsets/) | LeetCode | 🟡 Medium |
| [Subsets II](https://leetcode.com/problems/subsets-ii/) | LeetCode | 🟡 Medium |
