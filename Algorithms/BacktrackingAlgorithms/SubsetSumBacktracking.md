### **Subset Sum (Backtracking) 🔄**

Find **all subsets** of an array that sum to a target. Unlike the [DP version](../DPAlgorithms/SubsetSum.md) which only answers "is it possible?", backtracking can **enumerate** every valid subset.

---

#### **Summary**
- **Time Complexity:** O(2ⁿ) in the worst case (it explores all subsets)
- **Space Complexity:** O(n) recursion depth
- **Pruning:** stop early when running sum exceeds target (if values are positive)

---

#### **Python Implementation**
```python
def subset_sum_all(nums, target):
    results = []
    nums.sort()                   # enables pruning on sorted positives

    def backtrack(start, path, total):
        if total == target:
            results.append(list(path))
            return
        for i in range(start, len(nums)):
            if total + nums[i] > target:
                break             # prune (sorted)
            path.append(nums[i])
            backtrack(i + 1, path, total + nums[i])
            path.pop()

    backtrack(0, [], 0)
    return results

# Example
print(subset_sum_all([2, 3, 5, 6, 8, 10], 10))
# [[2, 3, 5], [2, 8], [10]]
```

---

#### **When to Use**
✅ Need actual subsets, not just yes/no
✅ Small `n` (≤ ~25) or strong pruning
🚫 Just feasibility on big inputs → use the DP version
