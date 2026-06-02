# Radix Sort 🔣

---

## 🧠 Intuition

Imagine sorting a pile of index cards by a 3-digit code. Instead of comparing full codes, you sort them **one digit at a time** — first by the ones digit (into 10 piles 0–9), then reassemble and sort by the tens digit, then by the hundreds digit. After 3 passes, all cards are in order.

**Mental model:** Sort digit by digit from least significant to most significant, using a stable sort for each digit pass.

---

## 📊 Complexity

| Case    | Time      | Space     | Stable | Adaptive |
|---------|-----------|-----------|--------|----------|
| Best    | O(n × d)  | O(n + k)  | ✅ Yes | ❌ No    |
| Average | O(n × d)  | O(n + k)  | ✅ Yes | ❌ No    |
| Worst   | O(n × d)  | O(n + k)  | ✅ Yes | ❌ No    |

> `n` = number of elements, `d` = number of digits in the max number, `k` = base (10 for decimal). When `d` is small (e.g., integers up to 10⁶ have d=7), this beats comparison sorts.

---

## ⚙️ How It Works

1. Find the **maximum number** to determine the number of digit passes needed.
2. For each digit position (ones, tens, hundreds, ...):
   - Use **Counting Sort** (stable!) to sort the array by that digit only.
3. After all digit passes, the array is fully sorted.

> The key insight: because Counting Sort is stable, the relative order from previous passes is preserved. After sorting by the ones digit and then the tens digit, numbers with the same tens digit maintain the order established by the ones pass.

---

## 🔢 Step-by-Step Trace

Input: `[170, 45, 75, 90, 802, 24, 2, 66]`

**Pass 1 — sort by ones digit:**
| Digit | Numbers |
|-------|---------|
| 0 | 170, 90 |
| 2 | 802, 2 |
| 4 | 24 |
| 5 | 45, 75 |
| 6 | 66 |

→ `[170, 90, 802, 2, 24, 45, 75, 66]`

**Pass 2 — sort by tens digit:**
| Digit | Numbers |
|-------|---------|
| 0 | 802, 2 |
| 2 | 24 |
| 4 | 45 |
| 6 | 66 |
| 7 | 170, 75 |
| 9 | 90 |

→ `[802, 2, 24, 45, 66, 170, 75, 90]`

**Pass 3 — sort by hundreds digit:**
→ `[2, 24, 45, 66, 75, 90, 170, 802]` ✅

---

## 🐍 Python Implementation

```python
def counting_sort_by_digit(arr, exp):
    """Stable sort of arr by the digit at position exp (1, 10, 100, ...)."""
    n = len(arr)
    output = [0] * n
    count = [0] * 10     # Digits 0–9

    # Count occurrences of each digit at this position
    for num in arr:
        digit = (num // exp) % 10
        count[digit] += 1

    # Make cumulative (each entry = number of elements ≤ this digit)
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Build output in reverse order to maintain stability
    for num in reversed(arr):
        digit = (num // exp) % 10
        count[digit] -= 1
        output[count[digit]] = num

    # Copy back to original array
    for i in range(n):
        arr[i] = output[i]

def radix_sort(arr):
    if not arr:
        return arr

    max_val = max(arr)       # Determine number of digit passes needed
    exp = 1                  # Start with ones digit
    while max_val // exp > 0:
        counting_sort_by_digit(arr, exp)
        exp *= 10            # Move to next digit (tens, hundreds, ...)

    return arr

# Example
arr = [170, 45, 75, 90, 802, 24, 2, 66]
print("Sorted:", radix_sort(arr))   # [2, 24, 45, 66, 75, 90, 170, 802]
```

---

## 🎯 Recognize This Problem When...

- You are sorting **large integers** with a bounded number of digits.
- You need to **beat O(n log n)** — Radix Sort runs in O(n × d).
- The data can be represented as **strings or fixed-width integers** (DNA sequences, IP addresses, dates).
- The number of digits `d` is much smaller than `n`.
- Sorting **strings lexicographically** of fixed length.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                      | Verdict                                    |
|------------------------------------------------|--------------------------------------------|
| Integers with a small number of digits         | ✅ Linear time — beats comparison sorts    |
| Sorting fixed-length strings                   | ✅ Each character position is one pass     |
| Numbers span a huge range with many digits     | ❌ d passes become expensive               |
| Floating-point numbers                         | ❌ Not directly applicable                 |
| Negative numbers (without modification)        | ❌ Handle separately (sort negatives then positives) |
| Small n (< 100)                               | ❌ Overhead not worth it; use QuickSort    |

---

## 🔗 Related Algorithms

| Algorithm                          | How it relates                                              |
|------------------------------------|-------------------------------------------------------------|
| [Counting Sort](CountingSort.md)   | Used as the sub-routine for each digit pass                 |
| [Bucket Sort](BucketSort.md)       | Also non-comparison; distributes by value range             |
| [Merge Sort](MergeSort.md)         | Best comparison sort alternative when radix isn't applicable|

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Maximum Gap                         | [LeetCode 164](https://leetcode.com/problems/maximum-gap/) | 🔴 Hard |
| Sort an Array                       | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Sort Characters By Frequency        | [LeetCode 451](https://leetcode.com/problems/sort-characters-by-frequency/) | 🟡 Medium |
