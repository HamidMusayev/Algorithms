# Interpolation Search 📐

---

## 🧠 Intuition

Imagine searching for the word "Ant" in a dictionary. You wouldn't open it to the middle — you'd open near the beginning, because "Ant" starts with A. Interpolation Search does the same: it **estimates** where the target is likely to be based on its value, rather than always jumping to the midpoint.

**Mental model:** If values are evenly spread, use proportion to guess the position — like guessing where "50" is in an array of [0, 10, 20, ..., 100].

---

## 📊 Complexity

| Case    | Time          | Space |
|---------|---------------|-------|
| Best    | O(1)          | O(1)  |
| Average | O(log log n)  | O(1)  |
| Worst   | O(n)          | O(1)  |

> O(log log n) average only holds for **uniformly distributed** data. For skewed or clustered data, performance degrades to O(n). For comparison, Binary Search is O(log n) always.

---

## ⚙️ How It Works

1. Use the **interpolation formula** to estimate position:
   ```
   pos = low + ((target - arr[low]) × (high - low)) / (arr[high] - arr[low])
   ```
2. If `arr[pos] == target` → found! Return `pos`.
3. If `arr[pos] < target` → search the **right** half: `low = pos + 1`.
4. If `arr[pos] > target` → search the **left** half: `high = pos - 1`.
5. If `low > high` or target is out of range → not found, return `-1`.

---

## 🔢 Step-by-Step Trace

**Array:** `[10, 20, 30, 40, 50, 60, 70, 80, 90]` (n=9) | **Target:** `70`

| Step | low | high | Estimated pos                                  | arr[pos] | Action           |
|------|-----|------|------------------------------------------------|----------|------------------|
| 1    | 0   | 8    | 0 + (70-10)×8/(90-10) = 0 + 60×8/80 = **6**  | 70       | **FOUND at index 6** |

One comparison! Binary search would take log₂(9) ≈ 3 comparisons. ✅

---

## 🐍 Python Implementation

```python
def interpolation_search(arr, target):
    """
    Searches for target in a SORTED, uniformly distributed array.
    Returns the index of target, or -1 if not found.
    """
    low, high = 0, len(arr) - 1

    while low <= high and arr[low] <= target <= arr[high]:
        # Guard against division by zero (all elements in range are equal)
        if arr[low] == arr[high]:
            if arr[low] == target:
                return low
            break

        # Interpolation formula: estimate where target lies proportionally
        pos = low + ((target - arr[low]) * (high - low) // (arr[high] - arr[low]))

        if arr[pos] == target:
            return pos              # Found!
        elif arr[pos] < target:
            low = pos + 1           # Target is in the right sub-array
        else:
            high = pos - 1          # Target is in the left sub-array

    return -1   # Not found


# Example
arr = [10, 20, 30, 40, 50, 60, 70, 80, 90]
print(interpolation_search(arr, 70))   # 6
print(interpolation_search(arr, 45))   # -1 (not in array)
print(interpolation_search(arr, 10))   # 0
```

---

## 🎯 Recognize This Problem When...

- The array is **sorted** AND values are **uniformly distributed** (e.g., IDs from 1000 to 9999, ages 0–100).
- You want to beat Binary Search's O(log n) on the right data.
- The problem hints at proportional or estimated lookup (interpolating position).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                           | Verdict                                    |
|-----------------------------------------------------|--------------------------------------------|
| Sorted array with uniformly distributed values      | ✅ O(log log n) — much better than Binary  |
| Searching sorted ages, uniform IDs, evenly-spaced data | ✅ Ideal                                |
| Data is NOT uniformly distributed (clustered, skewed) | ❌ Degrades to O(n) — use Binary Search  |
| Array contains duplicate values                     | ❌ Formula may produce wrong estimates     |
| Small arrays (n < 100)                              | ❌ Overhead outweighs benefit              |

---

## 🔗 Related Algorithms

| Algorithm                                  | How it relates                                         |
|--------------------------------------------|--------------------------------------------------------|
| [Binary Search](BinarySearch.md)           | Always O(log n) regardless of distribution — more reliable |
| [Linear Search](LinearSearch.md)           | No sorting required; O(n)                              |
| [Jump Search](JumpSearch.md)               | Also sorted-array search; O(√n) but no distribution assumption |
| [Exponential Search](ExponentialSearch.md) | Useful for unbounded arrays; uses Binary Search inside |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Search Insert Position              | [LeetCode 35](https://leetcode.com/problems/search-insert-position/) | 🟢 Easy |
| Binary Search                       | [LeetCode 704](https://leetcode.com/problems/binary-search/) | 🟢 Easy |
