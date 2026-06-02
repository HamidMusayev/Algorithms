# Selection Sort 🎯

---

## 🧠 Intuition

Imagine you have a row of numbered cards face down. You scan the entire row, find the smallest card, and place it at the front. Then you scan the remaining cards, find the next smallest, and place it second. You keep "selecting" the minimum from the remaining unsorted portion and dropping it into place.

**Mental model:** Repeatedly scan the unsorted region, pick the minimum, and drop it at the boundary.

---

## 📊 Complexity

| Case    | Time   | Space | Stable | Adaptive |
|---------|--------|-------|--------|----------|
| Best    | O(n²)  | O(1)  | ❌ No  | ❌ No    |
| Average | O(n²)  | O(1)  | ❌ No  | ❌ No    |
| Worst   | O(n²)  | O(1)  | ❌ No  | ❌ No    |

> **Not adaptive:** Selection Sort always performs exactly n(n-1)/2 comparisons regardless of the input order — it never short-circuits early.
> **Not stable by default:** Swapping non-adjacent elements can change the relative order of equal elements.

---

## ⚙️ How It Works

1. Divide the array conceptually into a **sorted left part** and an **unsorted right part**.
2. Initially, the sorted part is empty and the unsorted part is the whole array.
3. Scan the entire unsorted part to find the **index of the minimum element**.
4. **Swap** that minimum with the first element of the unsorted part.
5. Move the boundary one position to the right (the sorted part grows by one).
6. Repeat steps 3–5 until the unsorted part is empty.

---

## 🔢 Step-by-Step Trace

Input: `[64, 25, 12, 22, 11]`

| Pass | Array State                   | Action                                      |
|------|-------------------------------|---------------------------------------------|
| 0    | `[64, 25, 12, 22, 11]`       | Initial array                               |
| 1    | `[11, 25, 12, 22, 64]`       | Min=11 (index 4) swapped with index 0       |
| 2    | `[11, 12, 25, 22, 64]`       | Min=12 (index 2) swapped with index 1       |
| 3    | `[11, 12, 22, 25, 64]`       | Min=22 (index 3) swapped with index 2       |
| 4    | `[11, 12, 22, 25, 64]`       | Min=25 already at index 3, no swap needed   |
| Done | `[11, 12, 22, 25, 64]`       | Sorted ✅                                   |

---

## 🐍 Python Implementation

```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_index = i                   # Assume current position holds the minimum

        # Scan the unsorted portion to find the true minimum
        for j in range(i + 1, n):
            if arr[j] < arr[min_index]:
                min_index = j           # Update minimum index if a smaller value is found

        # Swap the found minimum into its correct position
        arr[i], arr[min_index] = arr[min_index], arr[i]

    return arr

# Example
arr = [64, 25, 12, 22, 11]
print("Sorted:", selection_sort(arr))   # [11, 12, 22, 25, 64]
```

---

## 🎯 Recognize This Problem When...

- You need to minimize the **number of writes/swaps** (Selection Sort does at most n-1 swaps — useful when memory writes are expensive).
- The dataset is **small** and you want the simplest possible sort.
- You are implementing sorting on **hardware with expensive memory writes** (e.g., flash memory).
- You want an **in-place** sort with no extra memory.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                      | Verdict                                      |
|------------------------------------------------|----------------------------------------------|
| Small arrays (n < 20)                          | ✅ Simple and effective                       |
| Minimizing number of swaps matters             | ✅ At most n-1 swaps                          |
| Large datasets                                 | ❌ O(n²) is too slow                         |
| Nearly sorted input                            | ❌ No adaptive benefit — still O(n²)         |
| Need stable sort                               | ❌ Not stable; use Insertion Sort or MergeSort|
| General-purpose sorting                        | ❌ Use QuickSort or MergeSort                 |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                   |
|----------------------------------------|------------------------------------------------------------------|
| [Bubble Sort](BubbleSort.md)           | Also O(n²), but stable; does more swaps                          |
| [Insertion Sort](InsertionSort.md)     | Also O(n²) but adaptive and stable; better for nearly sorted data|
| [Heap Sort](HeapSort.md)              | Generalizes Selection Sort using a heap to find the minimum in O(log n) instead of O(n) |
| [Merge Sort](MergeSort.md)             | O(n log n) stable sort — use for large data                      |

---

## 📝 Practice Problems

| Problem                           | Platform                                                                                   | Difficulty |
|-----------------------------------|--------------------------------------------------------------------------------------------|------------|
| Sort an Array                     | [LeetCode 912](https://leetcode.com/problems/sort-an-array/)                               | 🟡 Medium  |
| Minimum Number of Swaps to Sort   | [HackerRank](https://www.hackerrank.com/challenges/minimum-swaps-2/problem)                | 🟡 Medium  |
| Find the Minimum in an Array      | [LeetCode 153](https://leetcode.com/problems/find-minimum-in-rotated-sorted-array/)        | 🟡 Medium  |
| Selection Sort (classic)          | [HackerRank](https://www.hackerrank.com/challenges/quicksort1/problem)                     | 🟢 Easy    |
