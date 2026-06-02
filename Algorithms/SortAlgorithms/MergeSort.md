# Merge Sort 🔀

---

## 🧠 Intuition

Imagine sorting a deck of cards by splitting it into two halves, handing each half to a friend, and telling them to sort theirs. Once both friends are done, you merge the two sorted halves by repeatedly picking the smaller card from the top of either pile.

**Mental model:** Divide the problem in half recursively until trivial (1 element = sorted), then merge sorted halves — all the real work happens during the merge.

---

## 📊 Complexity

| Case    | Time        | Space  | Stable | Adaptive |
|---------|-------------|--------|--------|----------|
| Best    | O(n log n)  | O(n)   | ✅ Yes | ❌ No    |
| Average | O(n log n)  | O(n)   | ✅ Yes | ❌ No    |
| Worst   | O(n log n)  | O(n)   | ✅ Yes | ❌ No    |

> O(n) auxiliary space is used for the temporary arrays during merging. This is the key trade-off vs QuickSort.

---

## ⚙️ How It Works

1. **Base case:** If the array has 0 or 1 element, return it (already sorted).
2. **Divide:** Find the midpoint and split into `left` and `right` halves.
3. **Conquer:** Recursively sort both halves.
4. **Merge:** Combine the two sorted halves:
   - Compare front elements of both halves.
   - Always pick the smaller one and append to result.
   - When one half is exhausted, append all remaining elements from the other.

---

## 🔢 Step-by-Step Trace

Input: `[38, 27, 43, 3]`

```
Split phase:
[38, 27, 43, 3]
   /         \
[38, 27]   [43, 3]
 /    \     /    \
[38] [27] [43]  [3]

Merge phase:
[27, 38]   [3, 43]
     \       /
   [3, 27, 38, 43]
```

Merging `[27, 38]` and `[3, 43]`:

| Step | Left  | Right | Result so far       |
|------|-------|-------|---------------------|
| 1    | 27    | 3     | `[3]` ← pick 3      |
| 2    | 27    | 43    | `[3, 27]` ← pick 27 |
| 3    | 38    | 43    | `[3, 27, 38]` ← pick 38 |
| 4    | done  | 43    | `[3, 27, 38, 43]` ← append 43 |

---

## 🐍 Python Implementation

```python
def merge_sort(arr):
    if len(arr) <= 1:            # Base case: already sorted
        return arr

    mid = len(arr) // 2
    left = merge_sort(arr[:mid])     # Recursively sort left half
    right = merge_sort(arr[mid:])    # Recursively sort right half

    return merge(left, right)        # Merge the two sorted halves

def merge(left, right):
    result = []
    i = j = 0
    # Pick the smaller front element from each half until one is empty
    while i < len(left) and j < len(right):
        if left[i] <= right[j]:      # <= ensures stability (equal elements keep order)
            result.append(left[i])
            i += 1
        else:
            result.append(right[j])
            j += 1
    result.extend(left[i:])          # Append any remaining elements
    result.extend(right[j:])
    return result

# Example
arr = [38, 27, 43, 3, 9, 82, 10]
print("Sorted:", merge_sort(arr))    # [3, 9, 10, 27, 38, 43, 82]
```

---

## 🎯 Recognize This Problem When...

- You need a **guaranteed O(n log n)** sort (no worst-case O(n²) like QuickSort).
- You need a **stable sort** — equal elements must keep their original order.
- You are sorting a **linked list** (Merge Sort is preferred; QuickSort needs random access).
- The problem involves **counting inversions** in an array (count during merge step).
- Data is too large for RAM — **external merge sort** is the standard for disk-based sorting.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                    |
|--------------------------------------------|--------------------------------------------|
| Need guaranteed O(n log n) in all cases    | ✅ Merge Sort never degrades               |
| Need stable sort on large data             | ✅ Best stable O(n log n) sort             |
| Sorting linked lists                       | ✅ No random access needed                 |
| Counting inversions                        | ✅ Count during the merge step             |
| Memory is very constrained                 | ❌ Needs O(n) extra space                  |
| General-purpose in-memory sorting          | ❌ QuickSort is faster in practice         |

---

## 🔗 Related Algorithms

| Algorithm                          | How it relates                                              |
|------------------------------------|-------------------------------------------------------------|
| [Quick Sort](QuickSort.md)         | Also divide-and-conquer; faster average but O(n²) worst case |
| [Insertion Sort](InsertionSort.md) | Used as base case in TimSort (Python's built-in sort)       |
| [Heap Sort](HeapSort.md)           | Also O(n log n); in-place but not stable                    |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Sort an Array                       | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Count of Smaller Numbers After Self | [LeetCode 315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | 🔴 Hard |
| Sort List (Linked List)             | [LeetCode 148](https://leetcode.com/problems/sort-list/) | 🟡 Medium |
| Reverse Pairs                       | [LeetCode 493](https://leetcode.com/problems/reverse-pairs/) | 🔴 Hard |
