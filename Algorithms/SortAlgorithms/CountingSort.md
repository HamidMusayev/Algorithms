# Counting Sort 🔢

---

## 🧠 Intuition

Imagine sorting 1000 exam scores that are all between 0 and 100. Instead of comparing scores against each other, you just count how many students got each score. Then you reconstruct the sorted list by outputting each score the right number of times. No comparisons at all!

**Mental model:** Count occurrences of each value, then reconstruct the sorted array from those counts.

---

## 📊 Complexity

| Case    | Time      | Space  | Stable | Adaptive |
|---------|-----------|--------|--------|----------|
| Best    | O(n + k)  | O(k)   | ✅ Yes | ❌ No    |
| Average | O(n + k)  | O(k)   | ✅ Yes | ❌ No    |
| Worst   | O(n + k)  | O(k)   | ✅ Yes | ❌ No    |

> `n` = number of elements, `k` = range of input values (max value + 1). When `k` is much larger than `n`, this algorithm is wasteful.

---

## ⚙️ How It Works

1. Find the **maximum value** `k` in the array.
2. Create a **count array** of size `k + 1`, initialized to all zeros.
3. **Count:** For each element, increment `count[element]`.
4. **Cumulate** (for stable version): convert counts to cumulative counts so each value knows its position in the output.
5. **Build output:** Place each element at its correct position and decrement the count.

---

## 🔢 Step-by-Step Trace

Input: `[4, 2, 2, 8, 3, 3, 1]`, max = 8

**Count array** (index = value, value = frequency):

| Index | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 | 8 |
|-------|---|---|---|---|---|---|---|---|---|
| Count | 0 | 1 | 2 | 2 | 1 | 0 | 0 | 0 | 1 |

**Reconstruct sorted output** by reading counts:
- 1 appears 1 time → `[1]`
- 2 appears 2 times → `[1, 2, 2]`
- 3 appears 2 times → `[1, 2, 2, 3, 3]`
- 4 appears 1 time → `[1, 2, 2, 3, 3, 4]`
- 8 appears 1 time → `[1, 2, 2, 3, 3, 4, 8]`

Final: `[1, 2, 2, 3, 3, 4, 8]` ✅

---

## 🐍 Python Implementation

```python
def counting_sort(arr):
    if not arr:
        return arr

    max_val = max(arr)
    min_val = min(arr)
    range_val = max_val - min_val + 1

    count = [0] * range_val              # Count array for values in [min_val, max_val]

    for num in arr:
        count[num - min_val] += 1        # Offset by min_val to handle negative numbers

    # Reconstruct the sorted array from the count array
    sorted_arr = []
    for i, c in enumerate(count):
        sorted_arr.extend([i + min_val] * c)   # Output each value exactly c times

    return sorted_arr


def counting_sort_stable(arr):
    """Stable version that preserves original element order for equal values."""
    if not arr:
        return arr

    max_val = max(arr)
    count = [0] * (max_val + 1)

    for num in arr:
        count[num] += 1

    # Make cumulative: count[i] now holds the position AFTER the last occurrence of i
    for i in range(1, len(count)):
        count[i] += count[i - 1]

    output = [0] * len(arr)
    # Traverse in reverse to maintain stability
    for num in reversed(arr):
        count[num] -= 1
        output[count[num]] = num

    return output


# Example
arr = [4, 2, 2, 8, 3, 3, 1]
print("Sorted:", counting_sort(arr))         # [1, 2, 2, 3, 3, 4, 8]
print("Stable:", counting_sort_stable(arr))  # [1, 2, 2, 3, 3, 4, 8]
```

---

## 🎯 Recognize This Problem When...

- The values are **integers** (or can be mapped to integers).
- The **range `k` is small** relative to `n` — sorting a million ages (0–120) is perfect.
- You need to **beat O(n log n)** — comparison-based sorts can't go below it.
- The problem involves **sorting characters** (limited alphabet = small k).
- The problem uses Counting Sort as a sub-step (Radix Sort uses it digit by digit).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                      | Verdict                                    |
|------------------------------------------------|--------------------------------------------|
| Integer values in a small range                | ✅ Faster than any comparison sort         |
| Sorting exam scores, ages, vote counts         | ✅ Classic use case                        |
| n is huge but k is tiny                        | ✅ O(n + k) ≈ O(n)                        |
| k >> n (e.g., sort 10 numbers in range 0–10M) | ❌ Wastes memory and time                  |
| Negative numbers (without offset)             | ❌ Needs adjustment (use min offset)       |
| Floating-point values                         | ❌ Cannot use as index — use comparison sort|

---

## 🔗 Related Algorithms

| Algorithm                      | How it relates                                              |
|--------------------------------|-------------------------------------------------------------|
| [Radix Sort](RadixSort.md)     | Uses Counting Sort digit by digit for larger integers       |
| [Bucket Sort](BucketSort.md)   | Also non-comparison; distributes into buckets               |
| [Merge Sort](MergeSort.md)     | Best comparison sort alternative when range is large        |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Sort an Array                       | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Sort Colors                         | [LeetCode 75](https://leetcode.com/problems/sort-colors/) | 🟡 Medium |
| Maximum Gap                         | [LeetCode 164](https://leetcode.com/problems/maximum-gap/) | 🔴 Hard |
| Relative Sort Array                 | [LeetCode 1122](https://leetcode.com/problems/relative-sort-array/) | 🟢 Easy |
