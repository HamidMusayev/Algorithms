# Jump Search 🦘

---

## 🧠 Intuition

Imagine you're looking for a name in a printed phone book, but instead of checking every single entry (linear search) or flipping to the exact middle (binary search), you skip ahead 10 pages at a time. When you overshoot — the name on the current page is now alphabetically after your target — you go back one section and search that section line by line.

Jump search works exactly like this: leap forward in fixed-size blocks through a sorted array, then switch to a linear scan within the block where the target must live.

**Mental model:** Hop over blocks of √n elements until you overshoot, then walk backwards through the last block.

---

## 📊 Complexity

| Case    | Time     | Space |
|---------|----------|-------|
| Best    | O(1)     | O(1)  |
| Average | O(√n)    | O(1)  |
| Worst   | O(√n)    | O(1)  |

> The optimal block size is **√n**, which balances the number of jumps against the linear scan within the block. This makes worst-case O(√n) — better than O(n) linear search, but worse than O(log n) binary search.

---

## ⚙️ How It Works

1. Compute the step size: `step = floor(√n)` where n is the length of the array.
2. Start at index 0. Keep jumping forward by `step` until `arr[min(step, n) - 1] >= target` or you reach the end of the array.
3. Once you find the block where the target could exist (the end of the block is >= target), perform a **linear search** backwards from the current position to the start of the block.
4. If the element is found during the linear scan, return its index.
5. If the block is exhausted without finding the target, return `-1`.

---

## 🔢 Step-by-Step Trace

**Array:** `[2, 5, 8, 12, 16, 23, 38, 45, 56, 72]` (n=10) | **Target:** `23`  
**Step size:** `floor(√10) = 3`

| Phase  | Action                            | Indices Involved | Values Checked  | Result                         |
|--------|-----------------------------------|------------------|-----------------|--------------------------------|
| Jump 1 | Check arr[min(3,10)-1] = arr[2]  | 2                | 8               | 8 < 23 → jump again, prev=3   |
| Jump 2 | Check arr[min(6,10)-1] = arr[5]  | 5                | 23              | 23 >= 23 → stop jumping       |
| Scan 1 | Linear scan from index 3         | 3                | 12              | 12 ≠ 23, continue             |
| Scan 2 | Linear scan at index 4           | 4                | 16              | 16 ≠ 23, continue             |
| Scan 3 | Linear scan at index 5           | 5                | 23              | 23 == 23 → **FOUND at index 5** |

---

## 🐍 Python Implementation

```python
import math

def jump_search(arr, target):
    n = len(arr)
    step = int(math.sqrt(n))   # Optimal block size is √n
    prev = 0                   # Start of the current block

    # Phase 1: Jump forward until we find a block that could contain the target
    while prev < n and arr[min(step, n) - 1] < target:
        prev = step            # Move block start to current block end
        step += int(math.sqrt(n))  # Move block end forward by one step
        if prev >= n:
            return -1          # Jumped past the array — target not present

    # Phase 2: Linear search within the identified block
    for i in range(prev, min(step, n)):  # Scan from block start to block end (or array end)
        if arr[i] == target:
            return i           # Found — return the index
    return -1                  # Target not in this block


# --- Runnable Example ---
arr = [2, 5, 8, 12, 16, 23, 38, 45, 56, 72]
target = 23

result = jump_search(arr, target)
if result != -1:
    print(f"Element {target} found at index {result}")  # → index 5
else:
    print(f"Element {target} not found")
```

---

## 🎯 Recognize This Problem When...

- The array is **sorted** and you want something faster than linear but simpler than binary search.
- The data is stored in a way that **backward traversal is expensive** (e.g., magnetic tape — jump forward is cheap, but jumping back arbitrarily is costly, so you only ever go back by at most one block).
- You want **predictable, block-uniform access patterns** (cache-friendly jumping).
- The problem hints at a square-root decomposition approach.
- You're asked to balance between the number of large jumps and the fallback scan cost.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                          | Verdict                                                               |
|----------------------------------------------------|-----------------------------------------------------------------------|
| Sorted array, medium-to-large size                 | ✅ Efficient at O(√n), simple to implement                            |
| Backward traversal is expensive (e.g., tape drive) | ✅ Only steps back at most one block                                  |
| You need something between linear and binary       | ✅ A practical middle ground                                          |
| Array is unsorted                                  | ❌ Requires sorted input                                              |
| Maximum efficiency is required                     | ❌ Binary search at O(log n) is faster                               |
| Very small arrays (n < 10)                         | ❌ Linear search is simpler and just as fast                          |
| Random-access performance is critical              | ❌ Binary search makes fewer total comparisons                        |

---

## 🔗 Related Algorithms

| Algorithm                                        | How It Relates                                                                    |
|--------------------------------------------------|-----------------------------------------------------------------------------------|
| [LinearSearch](LinearSearch.md)                  | Used as the fallback scan within each block                                       |
| [BinarySearch](BinarySearch.md)                  | Faster alternative — O(log n) vs O(√n); could replace linear scan in each block |
| [ExponentialSearch](ExponentialSearch.md)        | Also jumps forward before a finer search, but uses doubling instead of fixed step |
| [InterpolationSearch](InterpolationSearch.md)    | Another sorted-array search that estimates position instead of jumping uniformly  |

---

## 📝 Practice Problems

| Problem                                         | Platform                                                                                     | Difficulty  |
|-------------------------------------------------|----------------------------------------------------------------------------------------------|-------------|
| Search Insert Position                          | [LeetCode #35](https://leetcode.com/problems/search-insert-position/)                       | 🟢 Easy     |
| Find Smallest Letter Greater Than Target        | [LeetCode #744](https://leetcode.com/problems/find-smallest-letter-greater-than-target/)    | 🟢 Easy     |
| Jump Search                                     | [HackerEarth](https://www.hackerearth.com/practice/algorithms/searching/binary-search/tutorial/) | 🟢 Easy |
| Sqrt(x)                                         | [LeetCode #69](https://leetcode.com/problems/sqrtx/)                                        | 🟢 Easy     |
| Find Peak Element                               | [LeetCode #162](https://leetcode.com/problems/find-peak-element/)                           | 🟡 Medium   |
