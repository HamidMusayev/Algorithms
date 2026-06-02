# Exponential Search 🚀

---

## 🧠 Intuition

Imagine you're looking for a word in a very long dictionary and you don't know how big it is. Instead of guessing the middle, you start small: check page 1, then page 2, then page 4, then page 8, then page 16 — doubling each time until you overshoot. Then you do Binary Search in the identified range.

**Mental model:** Double your range exponentially to bracket the target fast, then use Binary Search in the bracket.

---

## 📊 Complexity

| Case    | Time       | Space |
|---------|------------|-------|
| Best    | O(1)       | O(1)  |
| Average | O(log n)   | O(1)  |
| Worst   | O(log n)   | O(1)  |

> Same asymptotic complexity as Binary Search — but Exponential Search reaches the right range faster when the target is near the **beginning** of the array.

---

## ⚙️ How It Works

1. If the first element equals the target → return index 0.
2. Start at `i = 1`. Double `i` until `arr[i] > target` or `i >= n`.
3. Run **Binary Search** in the range `[i // 2, min(i, n - 1)]`.

---

## 🔢 Step-by-Step Trace

**Array:** `[2, 3, 4, 10, 40, 80, 120]` | **Target:** `10`

| Phase   | i   | arr[i] | Decision                          |
|---------|-----|--------|-----------------------------------|
| Exp     | 1   | 3      | 3 ≤ 10 → double                   |
| Exp     | 2   | 4      | 4 ≤ 10 → double                   |
| Exp     | 4   | 40     | 40 > 10 → stop                    |
| Binary  | Bin in `[2, 4]` → mid=3, arr[3]=10 → **FOUND at index 3** |

---

## 🐍 Python Implementation

```python
def binary_search(arr, left, right, target):
    """Standard iterative binary search within a range."""
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

def exponential_search(arr, target):
    """
    Search for target in a sorted array using Exponential Search.
    Returns the index of target, or -1 if not found.
    """
    n = len(arr)
    if n == 0:
        return -1

    # Special case: target is at position 0
    if arr[0] == target:
        return 0

    # Phase 1: Find range [i//2, i] where target could be
    i = 1
    while i < n and arr[i] <= target:
        i *= 2   # Double the index each step

    # Phase 2: Binary search within [i//2, min(i, n-1)]
    return binary_search(arr, i // 2, min(i, n - 1), target)


# Example
arr = [2, 3, 4, 10, 40, 80, 120]
print(exponential_search(arr, 10))    # 3
print(exponential_search(arr, 2))     # 0 (first element)
print(exponential_search(arr, 100))   # -1 (not found)
```

---

## 🎯 Recognize This Problem When...

- The array is **sorted** and potentially **very large or unbounded** (unknown length).
- The target is likely **near the beginning** of the array.
- You're searching in an **infinite sorted list** (like a stream or database).
- Jumping ahead exponentially is allowed but random access is expensive (limit total comparisons).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                       |
|--------------------------------------------|-----------------------------------------------|
| Target is near the start of a large array  | ✅ Faster than Binary Search in this case     |
| Unbounded / infinite sorted lists          | ✅ Naturally handles unknown array size       |
| Sorted array, target position unknown      | ✅ Finds range quickly, then binary searches  |
| Small arrays (n < 30)                      | ❌ Binary Search directly is simpler          |
| Unsorted arrays                            | ❌ Requires sorted input                      |
| Target is likely near the end              | ❌ Binary Search performs equally well        |

---

## 🔗 Related Algorithms

| Algorithm                                  | How it relates                                         |
|--------------------------------------------|--------------------------------------------------------|
| [Binary Search](BinarySearch.md)           | Used as the final step after the exponential phase     |
| [Jump Search](JumpSearch.md)               | Also jumps in blocks; fixed jump size vs exponential   |
| [Linear Search](LinearSearch.md)           | The fallback for unsorted arrays                       |
| [Interpolation Search](InterpolationSearch.md) | Estimates position using value; different strategy |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Binary Search                       | [LeetCode 704](https://leetcode.com/problems/binary-search/) | 🟢 Easy |
| Search in a Sorted Array of Unknown Size | [LeetCode 702](https://leetcode.com/problems/search-in-a-sorted-array-of-unknown-size/) | 🟡 Medium |
| Find First and Last Position of Element | [LeetCode 34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | 🟡 Medium |
