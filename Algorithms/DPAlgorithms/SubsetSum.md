# Subset Sum Problem ➕

---

## 🧠 Intuition

Given a set of numbers and a target sum, can you pick some of them (or none) to add up to exactly that target? It's like packing a bag to hit an exact weight limit — each item goes in or stays out.

The DP insight: `dp[i][s]` = "can we reach sum s using the first i numbers?" The answer only depends on whether we can reach s without item i, OR reach `s - nums[i]` without item i (and then include item i).

**Mental model:** Fill a boolean 2D table. True = reachable sum, False = unreachable.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n × target) |
| Space Complexity | O(n × target) — reducible to O(target) |

> NP-hard in general; pseudo-polynomial via DP. Works well when target is small.

---

## ⚙️ How It Works

**Recurrence:**
```
dp[i][s] = dp[i-1][s]                           if nums[i-1] > s  (can't include)
dp[i][s] = dp[i-1][s] OR dp[i-1][s - nums[i-1]] otherwise
```

Base cases:
- `dp[i][0] = True` for all i (empty subset always sums to 0)
- `dp[0][s] = False` for s > 0 (no numbers, can't reach non-zero sum)

---

## 🔢 Step-by-Step Trace

nums = `[3, 4, 2]`, target = 6

|       | 0    | 1    | 2    | 3    | 4    | 5    | 6    |
|-------|------|------|------|------|------|------|------|
| []    | ✅   | ❌   | ❌   | ❌   | ❌   | ❌   | ❌   |
| [3]   | ✅   | ❌   | ❌   | ✅   | ❌   | ❌   | ❌   |
| [3,4] | ✅   | ❌   | ❌   | ✅   | ✅   | ❌   | ❌   |
| [3,4,2]| ✅  | ❌   | ✅   | ✅   | ✅   | ✅   | ✅   |

`dp[3][6] = True` → subset {4, 2} or {3, 2+?}... wait: {4,2}=6 ✅

---

## 🐍 Python Implementation

```python
def subset_sum(nums, target):
    """Returns True if any subset of nums sums to target."""
    n = len(nums)
    # dp[i][s] = can we reach sum s using first i numbers
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        dp[i][0] = True    # Empty subset always sums to 0

    for i in range(1, n + 1):
        for s in range(1, target + 1):
            dp[i][s] = dp[i - 1][s]                          # Don't include nums[i-1]
            if nums[i - 1] <= s:                              # Can we include nums[i-1]?
                dp[i][s] = dp[i][s] or dp[i - 1][s - nums[i - 1]]

    return dp[n][target]


def subset_sum_optimized(nums, target):
    """Space-optimized O(target): use 1D boolean array, traverse backwards."""
    dp = [False] * (target + 1)
    dp[0] = True                           # Base: empty subset sums to 0

    for num in nums:
        # Traverse backwards to avoid using num twice in same "item round"
        for s in range(target, num - 1, -1):
            dp[s] = dp[s] or dp[s - num]

    return dp[target]


def subset_sum_backtrack(nums, target):
    """Find all subsets that sum to target (enumeration via backtracking)."""
    results = []
    nums.sort()

    def backtrack(idx, path, remaining):
        if remaining == 0:
            results.append(list(path))
            return
        for i in range(idx, len(nums)):
            if nums[i] > remaining:
                break                      # Pruning: sorted array, no point continuing
            path.append(nums[i])
            backtrack(i + 1, path, remaining - nums[i])
            path.pop()

    backtrack(0, [], target)
    return results


# Examples
nums = [3, 34, 4, 12, 5, 2]
print(subset_sum(nums, 9))            # True (4+5 or 3+4+2)
print(subset_sum(nums, 30))           # False

nums2 = [2, 3, 5, 6, 8, 10]
print(subset_sum_backtrack(nums2, 10))  # [[2, 3, 5], [2, 8], [10]]
```

---

## 🎯 Recognize This Problem When...

- You need to check if a **subset sums to a target** value.
- Keywords: "partition", "equal subset", "can we form amount", "target sum".
- The problem involves choosing a subset (each element 0 or 1 times).
- You see "split array into two equal halves" — that's subset sum with target = total/2.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Feasibility: can any subset hit the target? | ✅ Core use case |
| Partition problem (split into equal halves) | ✅ target = sum/2 |
| Items usable multiple times | ❌ Use Coin Change (unbounded) instead |
| Need all subsets, not just feasibility | ✅ Use backtracking variant |
| target is very large | ❌ O(n × target) too slow; consider meet-in-the-middle |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Knapsack](Knapsack.md) | Subset Sum = Knapsack where value = weight |
| [Coin Change](CoinChange.md) | Unbounded version: items reusable |
| [Subset Sum Backtracking](../BacktrackingAlgorithms/SubsetSumBacktracking.md) | Enumerate all valid subsets |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Partition Equal Subset Sum](https://leetcode.com/problems/partition-equal-subset-sum/) | LeetCode | 🟡 Medium |
| [Target Sum](https://leetcode.com/problems/target-sum/) | LeetCode | 🟡 Medium |
| [Sum of All Subset XOR Totals](https://leetcode.com/problems/sum-of-all-subset-xor-totals/) | LeetCode | 🟢 Easy |
| [Subarray Sum Equals K](https://leetcode.com/problems/subarray-sum-equals-k/) | LeetCode | 🟡 Medium |
