# Linear Search 🔦

---

## 🧠 Intuition

Imagine you lost your keys somewhere in your apartment. You walk room by room, checking every single spot in order until you find them — or until you've checked everywhere and accept they're gone. That's linear search: no shortcuts, no assumptions, just check each element one at a time from left to right.

**Mental model:** Walk the entire array, knocking on each door until the target answers.

---

## 📊 Complexity

| Case    | Time   | Space |
|---------|--------|-------|
| Best    | O(1)   | O(1)  |
| Average | O(n)   | O(1)  |
| Worst   | O(n)   | O(1)  |

> **Best case** occurs when the target is the very first element. **Worst case** occurs when the target is the last element or not present at all.

---

## ⚙️ How It Works

1. Start at index 0 (the first element of the array).
2. Compare the current element with the target value.
3. If they match, return the current index — the element is found.
4. If they don't match, move to the next element (increment index by 1).
5. Repeat steps 2–4 until the end of the array is reached.
6. If the end is reached without finding the target, return `-1` to indicate "not found".

---

## 🔢 Step-by-Step Trace

**Array:** `[10, 25, 7, 42, 31]` | **Target:** `42`

| Step | Index | Element Checked | Result            |
|------|-------|-----------------|-------------------|
| 1    | 0     | 10              | 10 ≠ 42, continue |
| 2    | 1     | 25              | 25 ≠ 42, continue |
| 3    | 2     | 7               | 7 ≠ 42, continue  |
| 4    | 3     | 42              | 42 == 42, **FOUND at index 3** |

---

## 🐍 Python Implementation

```python
def linear_search(arr, target):
    # Iterate through every index in the array
    for i in range(len(arr)):
        if arr[i] == target:       # Check if current element matches target
            return i               # Found — return the index immediately
    return -1                      # Exhausted the array without finding target


def linear_search_recursive(arr, target, index=0):
    if index >= len(arr):          # Base case: passed the end of the array
        return -1
    if arr[index] == target:       # Base case: current element matches target
        return index
    return linear_search_recursive(arr, target, index + 1)  # Check next element


# --- Runnable Example ---
arr = [10, 25, 7, 42, 31]
target = 42

result = linear_search(arr, target)
if result != -1:
    print(f"Iterative: Element {target} found at index {result}")   # → index 3
else:
    print(f"Iterative: Element {target} not found")

result_r = linear_search_recursive(arr, target)
if result_r != -1:
    print(f"Recursive: Element {target} found at index {result_r}") # → index 3
else:
    print(f"Recursive: Element {target} not found")
```

---

## 🎯 Recognize This Problem When...

- The array or list is **unsorted** and sorting it would be too costly.
- You need to **search in a linked list** (random access is impossible).
- The dataset is **very small** (n < 20) and overhead of sorting isn't worth it.
- You need to find **all occurrences** of a value (not just the first).
- There is **no ordering relationship** that can be exploited (e.g., searching objects by arbitrary fields).
- The problem says: *"find the element"* with no mention of the array being sorted.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                      | Verdict                                           |
|------------------------------------------------|---------------------------------------------------|
| Array is unsorted                              | ✅ Linear search is the right tool                |
| Array is small (n < ~20)                       | ✅ Overhead of sorting isn't justified            |
| Searching a linked list                        | ✅ No random access — linear is the only option   |
| Need to find all occurrences of a value        | ✅ Scan the full array naturally                  |
| Array is large (thousands of elements)         | ❌ Too slow — use Binary Search on sorted data    |
| Array is sorted                                | ❌ Binary Search is dramatically faster           |
| Performance-critical inner loop               | ❌ O(n) per query adds up quickly                 |

---

## 🔗 Related Algorithms

| Algorithm                                        | How It Relates                                                            |
|--------------------------------------------------|---------------------------------------------------------------------------|
| [BinarySearch](BinarySearch.md)                  | The sorted-array upgrade — O(log n) vs O(n)                              |
| [JumpSearch](JumpSearch.md)                      | Uses linear search as a fallback within each block                        |
| [InterpolationSearch](InterpolationSearch.md)    | Another sorted-array search; degrades to O(n) on non-uniform data        |
| [ExponentialSearch](ExponentialSearch.md)        | Starts with exponential jumps, then calls binary search within a range   |

---

## 📝 Practice Problems

| Problem                                      | Platform                                                                                  | Difficulty  |
|----------------------------------------------|-------------------------------------------------------------------------------------------|-------------|
| Search Insert Position                        | [LeetCode #35](https://leetcode.com/problems/search-insert-position/)                    | 🟢 Easy     |
| Find the Index of the First Occurrence        | [LeetCode #28](https://leetcode.com/problems/find-the-index-of-the-first-occurrence-in-a-string/) | 🟢 Easy |
| Linear Search                                 | [HackerRank](https://www.hackerrank.com/challenges/linear-search/problem)                | 🟢 Easy     |
| Find All Numbers Disappeared in an Array      | [LeetCode #448](https://leetcode.com/problems/find-all-numbers-disappeared-in-an-array/) | 🟢 Easy     |
| Contains Duplicate                            | [LeetCode #217](https://leetcode.com/problems/contains-duplicate/)                        | 🟢 Easy     |
