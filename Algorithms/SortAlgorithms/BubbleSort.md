# Bubble Sort 🔍

---

## 🧠 Intuition

Imagine a group of people standing in a line by height. You walk the line repeatedly and whenever two adjacent people are out of order, you swap them. The tallest person slowly **"bubbles up"** to the end — just like a bubble rising through water. You repeat until no more swaps happen.

**Mental model:** Every pass guarantees the largest unsorted element reaches its correct position at the end.

---

## 📊 Complexity

| Case         | Time       | Space  | Stable | Adaptive |
|--------------|------------|--------|--------|----------|
| Best         | O(n)       | O(1)   | ✅ Yes | ✅ Yes   |
| Average      | O(n²)      | O(1)   | ✅ Yes | ✅ Yes   |
| Worst        | O(n²)      | O(1)   | ✅ Yes | ✅ Yes   |

> **Best case O(n)** only applies when the `swapped` optimization is used — it detects an already-sorted array in one pass.

---

## ⚙️ How It Works

1. Start from the beginning of the array.
2. Compare each adjacent pair `(arr[j], arr[j+1])`.
3. If they are out of order → **swap** them.
4. After one full pass, the **largest element is in its final position**.
5. Repeat for the remaining unsorted portion.
6. **Optimization:** if no swaps happened in a pass → array is sorted → stop early.

---

## 🔢 Step-by-Step Trace

Input: `[5, 3, 1, 4, 2]`

| Pass | Array state after pass      | What happened                        |
|------|-----------------------------|--------------------------------------|
| 1    | `[3, 1, 4, 2, 5]`          | 5 bubbles up to the end              |
| 2    | `[1, 3, 2, 4, 5]`          | 4 settles, 3 moves right             |
| 3    | `[1, 2, 3, 4, 5]`          | 3 settles, array nearly sorted       |
| 4    | `[1, 2, 3, 4, 5]`          | No swaps → early exit ✅             |

---

## 🐍 Python Implementation

```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False                    # Optimization flag

        # Each pass shrinks the unsorted window by 1 (last i elements are sorted)
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:        # Out of order → swap
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True

        if not swapped:                    # No swaps = already sorted
            break

    return arr

# Example
arr = [5, 3, 1, 4, 2]
print("Sorted:", bubble_sort(arr))   # [1, 2, 3, 4, 5]
```

---

## 🎯 Recognize This Problem When...

- The problem involves **simple pairwise comparisons** and you need a quick, easy-to-implement sort.
- You are working with a **nearly sorted** array (great with the `swapped` optimization).
- You need a **stable sort** (equal elements keep their original relative order).
- You are in an **educational / interview explanation** context.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                 | Verdict         |
|-------------------------------------------|-----------------|
| Small arrays (n < 20)                     | ✅ Fine          |
| Nearly sorted input                        | ✅ Efficient     |
| Large datasets                            | ❌ Too slow      |
| Need best performance                     | ❌ Use QuickSort / MergeSort |
| Need stable sort on large data            | ❌ Use MergeSort |

---

## 🔗 Related Algorithms

| Algorithm                                      | How it relates                                      |
|------------------------------------------------|-----------------------------------------------------|
| [Selection Sort](SelectionSort.md)             | Also O(n²), but NOT stable; fewer swaps             |
| [Insertion Sort](InsertionSort.md)             | Also O(n²) but faster in practice for small/nearly-sorted data |
| [Merge Sort](MergeSort.md)                     | O(n log n), stable — use this for large data        |
| [Quick Sort](QuickSort.md)                     | O(n log n) average — fastest in practice            |

---

## 📝 Practice Problems

| Problem                        | Platform   | Difficulty |
|-------------------------------|------------|------------|
| Sort an Array                 | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Bubble Sort (classic)         | [HackerRank](https://www.hackerrank.com/challenges/ctci-bubble-sort/problem) | 🟢 Easy |
| Count Swaps in Bubble Sort    | [HackerRank](https://www.hackerrank.com/challenges/ctci-bubble-sort/problem) | 🟢 Easy |
