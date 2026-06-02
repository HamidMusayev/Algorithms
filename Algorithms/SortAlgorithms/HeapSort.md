# Heap Sort 🌳

---

## 🧠 Intuition

Imagine a tournament where the strongest player always rises to the top. You extract the champion, then re-run the tournament with the remaining players to find the next champion. Repeat until no players are left — they have been extracted in order of strength.

A **max-heap** is that tournament bracket — the root is always the largest element, and `heapify` re-runs the tournament after removal.

**Mental model:** Build a max-heap, then repeatedly extract the maximum and place it at the array's end.

---

## 📊 Complexity

| Case    | Time        | Space | Stable | Adaptive |
|---------|-------------|-------|--------|----------|
| Best    | O(n log n)  | O(1)  | ❌ No  | ❌ No    |
| Average | O(n log n)  | O(1)  | ❌ No  | ❌ No    |
| Worst   | O(n log n)  | O(1)  | ❌ No  | ❌ No    |

> **Guaranteed O(n log n) and in-place** — the best of both worlds compared to QuickSort (not guaranteed) and MergeSort (not in-place).

---

## ⚙️ How It Works

**Phase 1 — Build Max-Heap (heapify entire array):**
1. Start from the last non-leaf node (`n//2 - 1`) and call heapify down to index 0.
2. After this phase, `arr[0]` is the maximum.

**Phase 2 — Extract max one by one:**
3. Swap `arr[0]` (max) with `arr[i]` (last element in unsorted portion).
4. The swapped element is now in its **final sorted position**.
5. Reduce heap size by 1 and heapify from root to restore heap property.
6. Repeat until heap has 1 element.

---

## 🔢 Step-by-Step Trace

Input: `[4, 10, 3, 5, 1]`

**Build max-heap:**
- Heapify from index 1: 10 > children → no swap
- Heapify from index 0: 4 < 10 → swap → `[10, 5, 3, 4, 1]`

Heap: `[10, 5, 3, 4, 1]`

**Extract phase:**

| Step | Swap           | Heap portion        | Sorted portion  |
|------|----------------|---------------------|-----------------|
| 1    | arr[0]↔arr[4]  | heapify `[1,5,3,4]` → `[5,4,3,1]` | `[..., 10]` |
| 2    | arr[0]↔arr[3]  | heapify `[1,4,3]` → `[4,1,3]`     | `[..., 5, 10]`|
| 3    | arr[0]↔arr[2]  | heapify `[3,1]` → `[3,1]`         | `[..., 4, 5, 10]`|
| 4    | arr[0]↔arr[1]  | `[1]`                              | `[1, 3, 4, 5, 10]`|

Final: `[1, 3, 4, 5, 10]` ✅

---

## 🐍 Python Implementation

```python
def heap_sort(arr):
    n = len(arr)

    # Phase 1: Build a max-heap from the array
    # Start from last non-leaf and heapify upward to root
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # Phase 2: Extract elements one by one from the heap
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]   # Move current max to the end
        heapify(arr, i, 0)                 # Restore heap for the reduced heap

def heapify(arr, n, i):
    """Ensure the subtree rooted at index i satisfies max-heap property."""
    largest = i           # Assume root is largest
    left = 2 * i + 1
    right = 2 * i + 2

    if left < n and arr[left] > arr[largest]:    # Left child is larger than root
        largest = left
    if right < n and arr[right] > arr[largest]:  # Right child is largest of all
        largest = right

    if largest != i:                             # Root is not the largest
        arr[i], arr[largest] = arr[largest], arr[i]  # Swap root with largest
        heapify(arr, n, largest)                 # Recursively fix the affected subtree

# Example
arr = [4, 10, 3, 5, 1]
heap_sort(arr)
print("Sorted:", arr)   # [1, 3, 4, 5, 10]
```

---

## 🎯 Recognize This Problem When...

- You need **in-place O(n log n)** sorting with no extra memory.
- You need a **priority queue** — max/min element always accessible in O(log n).
- The problem asks for the **k largest/smallest** elements efficiently.
- You need worst-case O(n log n) AND in-place (QuickSort fails the guarantee; MergeSort fails in-place).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                          |
|--------------------------------------------|--------------------------------------------------|
| In-place O(n log n) with no extra space    | ✅ Only O(n log n) in-place sort with guarantee  |
| k largest/smallest elements                | ✅ Use a heap of size k                          |
| Memory is very constrained                 | ✅ O(1) auxiliary space                          |
| Need stable sort                           | ❌ Not stable — use Merge Sort                   |
| Cache performance matters                  | ❌ QuickSort is faster in practice (cache-friendly)|
| Nearly sorted input                        | ❌ No adaptive benefit                           |

---

## 🔗 Related Algorithms

| Algorithm                          | How it relates                                              |
|------------------------------------|-------------------------------------------------------------|
| [Quick Sort](QuickSort.md)         | Also in-place; faster average case but O(n²) worst case    |
| [Merge Sort](MergeSort.md)         | Guaranteed O(n log n) and stable; needs O(n) space         |
| [Selection Sort](SelectionSort.md) | Same "find max, place at end" idea — Heap Sort is the O(n log n) upgrade |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Kth Largest Element in an Array     | [LeetCode 215](https://leetcode.com/problems/kth-largest-element-in-an-array/) | 🟡 Medium |
| Sort an Array                       | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Top K Frequent Elements             | [LeetCode 347](https://leetcode.com/problems/top-k-frequent-elements/) | 🟡 Medium |
| Find Median from Data Stream        | [LeetCode 295](https://leetcode.com/problems/find-median-from-data-stream/) | 🔴 Hard |
