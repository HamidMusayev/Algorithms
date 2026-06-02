# Binary Search 🔎

---

## 🧠 Intuition

Think of searching for a word in a physical dictionary. You don't start from page 1 — you flip to the middle, see if your word comes before or after, then flip to the middle of the remaining half. Each flip eliminates half the book. After just a few flips you're on the right page.

Binary search applies this exact strategy to a sorted array: each comparison cuts the search space in half, giving you a massively efficient O(log n) algorithm.

**Mental model:** Open the sorted book in the middle, throw away the half that can't contain the answer, repeat.

---

## 📊 Complexity

| Case    | Time      | Space |
|---------|-----------|-------|
| Best    | O(1)      | O(1)  |
| Average | O(log n)  | O(1)  |
| Worst   | O(log n)  | O(1)  |

> **Best case** is when the target happens to be at the very first midpoint. Space is O(1) for the iterative version; the recursive version uses O(log n) stack space.

---

## ⚙️ How It Works

1. Set two pointers: `left = 0` and `right = len(array) - 1`.
2. While `left <= right`:
   a. Compute `mid = left + (right - left) // 2` (avoids integer overflow).
   b. If `arr[mid] == target` → found! Return `mid`.
   c. If `arr[mid] < target` → target must be in the right half; set `left = mid + 1`.
   d. If `arr[mid] > target` → target must be in the left half; set `right = mid - 1`.
3. If the loop ends without returning, the target is not in the array; return `-1`.

---

## 🔢 Step-by-Step Trace

**Array:** `[3, 9, 15, 22, 34, 47, 58]` | **Target:** `34`

| Step | left | right | mid | arr[mid] | Decision                    |
|------|------|-------|-----|----------|-----------------------------|
| 1    | 0    | 6     | 3   | 22       | 22 < 34 → search right half, left = 4 |
| 2    | 4    | 6     | 5   | 47       | 47 > 34 → search left half, right = 4 |
| 3    | 4    | 4     | 4   | 34       | 34 == 34 → **FOUND at index 4** |

---

## 🐍 Python Implementation

```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1    # Initialize search boundaries

    while left <= right:              # Continue while there's a valid search range
        # Use this formula to avoid integer overflow (important in lower-level languages)
        mid = left + (right - left) // 2

        if arr[mid] == target:
            return mid                # Target found — return its index
        elif arr[mid] < target:
            left = mid + 1            # Target is in the right half — discard left
        else:
            right = mid - 1           # Target is in the left half — discard right

    return -1                         # Search space exhausted — target not present


def binary_search_recursive(arr, target, left, right):
    if left > right:                  # Base case: empty search range
        return -1

    mid = left + (right - left) // 2  # Midpoint of current range

    if arr[mid] == target:
        return mid                    # Base case: found the target
    elif arr[mid] < target:
        return binary_search_recursive(arr, target, mid + 1, right)  # Go right
    else:
        return binary_search_recursive(arr, target, left, mid - 1)   # Go left


# --- Runnable Example ---
arr = [3, 9, 15, 22, 34, 47, 58]
target = 34

result = binary_search(arr, target)
if result != -1:
    print(f"Iterative: Element {target} found at index {result}")      # → index 4
else:
    print(f"Iterative: Element {target} not found")

result_r = binary_search_recursive(arr, target, 0, len(arr) - 1)
if result_r != -1:
    print(f"Recursive: Element {target} found at index {result_r}")    # → index 4
else:
    print(f"Recursive: Element {target} not found")
```

---

## 🎯 Recognize This Problem When...

- The problem explicitly says the array is **sorted**.
- You see the phrase **"find in O(log n)"** or **"efficiently search"**.
- The problem involves finding a **boundary** (first/last position, minimum valid value).
- You need to **search for a condition** that splits true/false across a monotonic range (binary search on answer).
- The input size is large (n > 10,000) and a linear scan would time out.
- Problems involving **rotated sorted arrays**, **peak elements**, or **square roots** often hide binary search.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                     | Verdict                                                         |
|-----------------------------------------------|-----------------------------------------------------------------|
| Array is sorted                               | ✅ Binary search is the standard approach                       |
| Large dataset, need fast lookup               | ✅ O(log n) is extremely efficient at scale                     |
| Finding first/last occurrence of a value      | ✅ Extend with left/right boundary binary search                |
| Binary search on the answer space             | ✅ Works when valid/invalid answers form a monotone sequence    |
| Array is unsorted and sorting is too costly   | ❌ Use linear search instead                                    |
| Data structure doesn't support random access  | ❌ Binary search needs O(1) index access (not linked lists)     |
| Dataset is tiny (n < 10)                      | ❌ Overhead isn't worth it; linear search is simpler            |

---

## 🔗 Related Algorithms

| Algorithm                                        | How It Relates                                                            |
|--------------------------------------------------|---------------------------------------------------------------------------|
| [LinearSearch](LinearSearch.md)                  | The unsorted alternative — simple but O(n)                               |
| [JumpSearch](JumpSearch.md)                      | Middle ground: jumps in blocks, then does linear scan                    |
| [ExponentialSearch](ExponentialSearch.md)        | Uses binary search internally after finding the right range              |
| [InterpolationSearch](InterpolationSearch.md)    | An improvement on binary search for uniformly distributed data           |

---

## 📝 Practice Problems

| Problem                                             | Platform                                                                                         | Difficulty  |
|-----------------------------------------------------|--------------------------------------------------------------------------------------------------|-------------|
| Binary Search                                       | [LeetCode #704](https://leetcode.com/problems/binary-search/)                                   | 🟢 Easy     |
| Search Insert Position                              | [LeetCode #35](https://leetcode.com/problems/search-insert-position/)                           | 🟢 Easy     |
| Find First and Last Position of Element in Sorted Array | [LeetCode #34](https://leetcode.com/problems/find-first-and-last-position-of-element-in-sorted-array/) | 🟡 Medium |
| Search in Rotated Sorted Array                      | [LeetCode #33](https://leetcode.com/problems/search-in-rotated-sorted-array/)                   | 🟡 Medium   |
| Find Minimum in Rotated Sorted Array                | [LeetCode #153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)            | 🟡 Medium   |
| Median of Two Sorted Arrays                         | [LeetCode #4](https://leetcode.com/problems/median-of-two-sorted-arrays/)                       | 🔴 Hard     |
| Ice Cream Parlor                                    | [HackerRank](https://www.hackerrank.com/challenges/icecream-parlor/problem)                     | 🟡 Medium   |
