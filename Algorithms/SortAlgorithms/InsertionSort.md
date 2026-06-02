# Insertion Sort 🃏

---

## 🧠 Intuition

Think of sorting a hand of playing cards. You pick up cards one at a time from the table. For each new card, you slide it left through your already-sorted hand until it's in the right spot. The left portion of your hand is always sorted; you just keep inserting the next card into the correct position.

**Mental model:** Build a sorted array one element at a time by inserting each new element into its correct position among the already-sorted elements.

---

## 📊 Complexity

| Case    | Time   | Space | Stable | Adaptive |
|---------|--------|-------|--------|----------|
| Best    | O(n)   | O(1)  | ✅ Yes | ✅ Yes   |
| Average | O(n²)  | O(1)  | ✅ Yes | ✅ Yes   |
| Worst   | O(n²)  | O(1)  | ✅ Yes | ✅ Yes   |

> **Best case O(n):** When the array is already sorted, each element only needs one comparison and no shifts — the inner loop never executes.
> **Adaptive:** Fewer inversions in the input means fewer total operations.

---

## ⚙️ How It Works

1. Start with the **second element** (index 1); the first element is trivially sorted on its own.
2. Save the current element as `key`.
3. Compare `key` with each element to its **left**, moving right-to-left.
4. **Shift** each element that is greater than `key` one position to the right.
5. When you find an element ≤ `key` (or reach the start), **insert** `key` in the gap.
6. Move to the next element and repeat.

---

## 🔢 Step-by-Step Trace

Input: `[12, 11, 13, 5, 6]`

| Step | Key | Array State             | What happened                              |
|------|-----|-------------------------|--------------------------------------------|
| 0    | —   | `[12, 11, 13, 5, 6]`   | Initial                                    |
| 1    | 11  | `[11, 12, 13, 5, 6]`   | 11 < 12 → shift 12 right, insert 11 at 0  |
| 2    | 13  | `[11, 12, 13, 5, 6]`   | 13 > 12 → already in place, no shift      |
| 3    | 5   | `[5, 11, 12, 13, 6]`   | 5 < 13, 12, 11, 12 → shift all, insert at 0|
| 4    | 6   | `[5, 6, 11, 12, 13]`   | 6 < 13, 12, 11 → shift, insert after 5    |
| Done | —   | `[5, 6, 11, 12, 13]`   | Sorted ✅                                  |

---

## 🐍 Python Implementation

```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]        # The element we want to insert into the sorted portion
        j = i - 1          # Start comparing from the element just before key

        # Shift elements of arr[0..i-1] that are greater than key one position right
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]     # Shift element right to make room
            j -= 1                  # Move one step left

        arr[j + 1] = key    # Insert key into its correct sorted position

    return arr

# Example
arr = [12, 11, 13, 5, 6]
print("Sorted:", insertion_sort(arr))   # [5, 6, 11, 12, 13]
```

---

## 🎯 Recognize This Problem When...

- The input is **nearly sorted** (few inversions) — Insertion Sort excels here.
- You are sorting a **small array** (n < 20) where overhead of complex algorithms isn't worth it.
- Data arrives **one piece at a time** (online sorting) and must stay sorted as new elements come in.
- You need a **stable sort** with **no extra memory**.
- You see an interview problem where simplicity + stability + in-place is required.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                      | Verdict                                          |
|------------------------------------------------|--------------------------------------------------|
| Small arrays (n < 20)                          | ✅ Fast and simple                               |
| Nearly sorted input                            | ✅ Very efficient — close to O(n)                |
| Online / streaming data                        | ✅ Insert new elements in O(n)                   |
| Large, random datasets                         | ❌ O(n²) is too slow                             |
| Need best average performance                  | ❌ Use QuickSort or MergeSort                    |
| Used inside Timsort / Hybrid sorts             | ✅ Used for small runs (Python's built-in sort)   |

---

## 🔗 Related Algorithms

| Algorithm                              | How it relates                                                        |
|----------------------------------------|-----------------------------------------------------------------------|
| [Bubble Sort](BubbleSort.md)           | Also O(n²) and stable, but slower in practice (more swaps)            |
| [Selection Sort](SelectionSort.md)     | Also O(n²) but NOT stable and NOT adaptive                            |
| [Shell Sort](ShellSort.md)            | Generalization of Insertion Sort using larger gaps to speed it up     |
| [Merge Sort](MergeSort.md)             | O(n log n) stable sort; Timsort uses Insertion Sort for small chunks  |
| [Quick Sort](QuickSort.md)             | O(n log n) average; not stable but much faster on large random data   |

---

## 📝 Practice Problems

| Problem                              | Platform                                                                                       | Difficulty |
|--------------------------------------|------------------------------------------------------------------------------------------------|------------|
| Sort an Array                        | [LeetCode 912](https://leetcode.com/problems/sort-an-array/)                                   | 🟡 Medium  |
| Insertion Sort Part 1                | [HackerRank](https://www.hackerrank.com/challenges/insertionsort1/problem)                     | 🟢 Easy    |
| Insertion Sort Part 2                | [HackerRank](https://www.hackerrank.com/challenges/insertionsort2/problem)                     | 🟢 Easy    |
| Count of Smaller Numbers After Self  | [LeetCode 315](https://leetcode.com/problems/count-of-smaller-numbers-after-self/)             | 🔴 Hard    |
| Insert into a Sorted Circular List   | [LeetCode 708](https://leetcode.com/problems/insert-into-a-sorted-circular-linked-list/)       | 🟡 Medium  |
