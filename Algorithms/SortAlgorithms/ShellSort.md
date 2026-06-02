# Shell Sort 🐚

---

## 🧠 Intuition

Insertion Sort is great for nearly-sorted arrays but slow when small elements are far from the front (they have to travel one step at a time). Shell Sort solves this by first doing insertion sort with a **large gap** — letting elements leap across large distances — then gradually reducing the gap until it reaches 1 (a final regular insertion sort pass).

**Mental model:** Pre-sort the array at large gaps so the final Insertion Sort pass has very little work to do.

---

## 📊 Complexity

| Case    | Time                       | Space | Stable | Adaptive |
|---------|----------------------------|-------|--------|----------|
| Best    | O(n log n)                 | O(1)  | ❌ No  | ✅ Yes   |
| Average | O(n^1.5) to O(n log² n)    | O(1)  | ❌ No  | ✅ Yes   |
| Worst   | O(n²) with shell gaps      | O(1)  | ❌ No  | ✅ Yes   |

> Complexity depends heavily on the **gap sequence**. Shell's original sequence (n/2, n/4, ..., 1) gives O(n²) worst case. Ciura's sequence (1, 4, 10, 23, 57, 132, 301, 701, ...) gives O(n^(4/3)) in practice.

---

## ⚙️ How It Works

1. Choose a starting **gap** (commonly `n // 2`).
2. Perform **gapped Insertion Sort**: for each element, compare it with the element `gap` positions before it and shift if out of order.
3. **Halve the gap** and repeat.
4. When `gap = 1`, this is a standard Insertion Sort pass — but the array is nearly sorted now, so it finishes quickly.

---

## 🔢 Step-by-Step Trace

Input: `[12, 34, 54, 2, 3]`, n=5

**Gap = 2** (compare elements 2 apart):
- Compare arr[2]=54 vs arr[0]=12 → OK
- Compare arr[3]=2 vs arr[1]=34 → swap → `[12, 2, 54, 34, 3]`
- Compare arr[4]=3 vs arr[2]=54 → swap → `[12, 2, 3, 34, 54]`
  - Also compare arr[2]=3 vs arr[0]=12 → swap → `[3, 2, 12, 34, 54]`

After gap=2: `[3, 2, 12, 34, 54]`

**Gap = 1** (standard insertion sort — nearly sorted!):
- 2 < 3 → swap → `[2, 3, 12, 34, 54]`
- All others already in place.

Final: `[2, 3, 12, 34, 54]` ✅

---

## 🐍 Python Implementation

```python
def shell_sort(arr):
    n = len(arr)
    gap = n // 2            # Start with half the array length as the gap

    while gap > 0:
        # Gapped insertion sort for this gap size
        for i in range(gap, n):
            temp = arr[i]   # Element to be inserted into the correct position
            j = i
            # Shift elements that are greater than temp by gap positions
            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]    # Shift element right by gap positions
                j -= gap
            arr[j] = temp               # Insert element in its correct gap-sorted position
        gap //= 2           # Reduce gap — eventually reaches 1 (final Insertion Sort)

    return arr


def shell_sort_ciura(arr):
    """Shell Sort with Ciura's empirically optimized gap sequence."""
    n = len(arr)
    # Ciura's sequence (best known practical gap sequence)
    gaps = [1, 4, 10, 23, 57, 132, 301, 701]
    gaps = [g for g in gaps if g < n]   # Only use gaps smaller than array size

    for gap in reversed(gaps):          # Start with largest gap
        for i in range(gap, n):
            temp = arr[i]
            j = i
            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]
                j -= gap
            arr[j] = temp

    return arr


# Example
arr = [12, 34, 54, 2, 3]
print("Sorted:", shell_sort(arr))       # [2, 3, 12, 34, 54]

arr2 = [64, 25, 12, 22, 11, 90, 45]
print("Ciura:", shell_sort_ciura(arr2)) # [11, 12, 22, 25, 45, 64, 90]
```

---

## 🎯 Recognize This Problem When...

- You need an **in-place sort** that is faster than O(n²) on average without complex logic.
- Memory is constrained (O(1) space) but you want better performance than simple Insertion Sort.
- You need a sort that works well on **medium-sized arrays** (n = 100 to 10,000).
- You want a simple implementation that beats Insertion Sort significantly.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                      | Verdict                                    |
|------------------------------------------------|--------------------------------------------|
| Medium arrays (n = 100–10,000)                 | ✅ Faster than Insertion/Bubble/Selection  |
| Memory-constrained in-place sorting            | ✅ O(1) extra space, no recursion          |
| Simple implementation needed                   | ✅ Easier to code than QuickSort/MergeSort |
| Very large datasets                            | ❌ Use QuickSort or MergeSort              |
| Need stable sort                               | ❌ Not stable — use Merge Sort             |
| Need theoretical guarantees                    | ❌ Complexity depends on gap sequence      |

---

## 🔗 Related Algorithms

| Algorithm                          | How it relates                                              |
|------------------------------------|-------------------------------------------------------------|
| [Insertion Sort](InsertionSort.md) | Shell Sort IS Insertion Sort but with varying gap sizes     |
| [Quick Sort](QuickSort.md)         | Better average-case in-place sort for large data            |
| [Merge Sort](MergeSort.md)         | Guaranteed O(n log n) with O(n) space                       |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Sort an Array                       | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Insertion Sort List                 | [LeetCode 147](https://leetcode.com/problems/insertion-sort-list/) | 🟡 Medium |
