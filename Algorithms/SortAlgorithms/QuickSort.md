# Quick Sort ⚡

---

## 🧠 Intuition

Imagine organizing books on a shelf. Pick one book as a reference ("pivot"). Put all shorter books to its left and all taller books to its right. Now do the same recursively for the left group and the right group. You never need to compare books across the pivot boundary.

**Mental model:** Pick a pivot → partition around it (pivot lands in its final position) → recurse on each side.

---

## 📊 Complexity

| Case    | Time        | Space     | Stable | Adaptive |
|---------|-------------|-----------|--------|----------|
| Best    | O(n log n)  | O(log n)  | ❌ No  | ❌ No    |
| Average | O(n log n)  | O(log n)  | ❌ No  | ❌ No    |
| Worst   | O(n²)       | O(n)      | ❌ No  | ❌ No    |

> Worst case O(n²) happens when pivot is always the min/max (e.g., sorted array with first/last pivot). Use **randomized pivot** to make this astronomically unlikely in practice.

---

## ⚙️ How It Works

1. Choose a **pivot** element (last element, random, or median-of-three).
2. **Partition:** Rearrange so that:
   - All elements **≤ pivot** are on its left.
   - All elements **> pivot** are on its right.
   - The pivot is now in its **final sorted position**.
3. Recursively QuickSort the **left sub-array** (elements < pivot).
4. Recursively QuickSort the **right sub-array** (elements > pivot).
5. **Base case:** Sub-array of size 0 or 1 is already sorted — stop.

---

## 🔢 Step-by-Step Trace

Input: `[10, 80, 30, 90, 40]`, pivot = last element = `40`

**Partition scan** (i tracks boundary of "≤ pivot" region):

| j | arr[j] | ≤ 40? | Action                   | Array            |
|---|--------|-------|--------------------------|------------------|
| 0 | 10     | ✅    | i=0, swap arr[0]↔arr[0] | `[10,80,30,90,40]` |
| 1 | 80     | ❌    | skip                     | unchanged        |
| 2 | 30     | ✅    | i=1, swap arr[1]↔arr[2] | `[10,30,80,90,40]` |
| 3 | 90     | ❌    | skip                     | unchanged        |

Place pivot: swap arr[i+1]=arr[2]↔arr[4] → `[10, 30, 40, 90, 80]`

Pivot `40` is now at index 2 ✅ (its final position)

Recurse left `[10, 30]` → already sorted  
Recurse right `[90, 80]` → `[80, 90]`  
Final: `[10, 30, 40, 80, 90]`

---

## 🐍 Python Implementation

```python
import random

def quick_sort(arr, low=0, high=None):
    if high is None:
        high = len(arr) - 1
    if low < high:
        pi = partition(arr, low, high)   # pi = pivot's final index
        quick_sort(arr, low, pi - 1)     # Sort elements left of pivot
        quick_sort(arr, pi + 1, high)    # Sort elements right of pivot

def partition(arr, low, high):
    # Randomize pivot to avoid worst-case on sorted arrays
    rand_idx = random.randint(low, high)
    arr[rand_idx], arr[high] = arr[high], arr[rand_idx]

    pivot = arr[high]       # Pivot is now at the last position
    i = low - 1             # i marks the right edge of the "≤ pivot" region

    for j in range(low, high):
        if arr[j] <= pivot:              # Current element belongs in left region
            i += 1
            arr[i], arr[j] = arr[j], arr[i]   # Swap into left region

    arr[i + 1], arr[high] = arr[high], arr[i + 1]  # Place pivot in final position
    return i + 1            # Return pivot's index

# Example
arr = [10, 80, 30, 90, 40, 50, 70]
quick_sort(arr)
print("Sorted:", arr)   # [10, 30, 40, 50, 70, 80, 90]
```

---

## 🎯 Recognize This Problem When...

- You need the **fastest average-case** in-place sort.
- You need to find the **k-th smallest/largest** element (QuickSelect — O(n) average).
- Memory is constrained and you cannot afford O(n) extra space.
- The problem has random or diverse data (worst case is very unlikely with random pivot).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                  | Verdict                                        |
|--------------------------------------------|------------------------------------------------|
| General-purpose in-memory sorting          | ✅ Fastest in practice (cache-friendly)        |
| Finding k-th smallest element              | ✅ QuickSelect runs in O(n) average            |
| In-place sorting with limited memory       | ✅ O(log n) stack space only                   |
| Need guaranteed worst-case O(n log n)      | ❌ Use Merge Sort or Heap Sort                 |
| Need stable sort                           | ❌ Not stable — use Merge Sort                 |
| Already sorted / reverse sorted array      | ❌ Use randomized pivot or Merge Sort          |

---

## 🔗 Related Algorithms

| Algorithm                          | How it relates                                              |
|------------------------------------|-------------------------------------------------------------|
| [Merge Sort](MergeSort.md)         | Stable, O(n log n) guaranteed; uses O(n) extra space        |
| [Heap Sort](HeapSort.md)           | Also in-place O(n log n) worst case; slower in practice     |
| [Insertion Sort](InsertionSort.md) | Used as base case in hybrid QuickSort for small sub-arrays  |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Sort an Array                       | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Kth Largest Element in an Array     | [LeetCode 215](https://leetcode.com/problems/kth-largest-element-in-an-array/) | 🟡 Medium |
| Sort Colors (Dutch National Flag)   | [LeetCode 75](https://leetcode.com/problems/sort-colors/) | 🟡 Medium |
| Wiggle Sort II                      | [LeetCode 324](https://leetcode.com/problems/wiggle-sort-ii/) | 🟡 Medium |
