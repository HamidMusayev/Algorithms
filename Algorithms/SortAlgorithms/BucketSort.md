# Bucket Sort 🪣

---

## 🧠 Intuition

Imagine sorting 1000 people by age. Instead of comparing everyone directly, you set up 10 buckets labeled "0–9", "10–19", "20–29", etc. Each person steps into their bucket. Then you sort within each bucket (very few people per bucket) and concatenate the buckets in order.

**Mental model:** Distribute elements into range-based buckets, sort each small bucket cheaply, then concatenate.

---

## 📊 Complexity

| Case    | Time        | Space     | Stable | Adaptive |
|---------|-------------|-----------|--------|----------|
| Best    | O(n + k)    | O(n + k)  | ✅ Yes | ❌ No    |
| Average | O(n + k)    | O(n + k)  | ✅ Yes | ❌ No    |
| Worst   | O(n²)       | O(n + k)  | ✅ Yes | ❌ No    |

> Worst case O(n²) occurs when all elements fall into the same bucket. With **uniformly distributed data**, average case approaches O(n). `k` = number of buckets.

---

## ⚙️ How It Works

1. Determine the **range** of input values.
2. Create `k` empty buckets, each covering an equal sub-range.
3. **Distribute:** Place each element into the appropriate bucket based on its value.
4. **Sort each bucket** individually (typically with Insertion Sort — buckets are small).
5. **Concatenate** all buckets in order to produce the sorted output.

---

## 🔢 Step-by-Step Trace

Input: `[0.42, 0.32, 0.23, 0.52, 0.25, 0.47]` — floats in `[0, 1)`, 6 buckets

**Distribution** (bucket index = `floor(value × n)` where n=6):

| Value | Bucket Index | Bucket Contents After |
|-------|-------------|----------------------|
| 0.42  | 2           | bucket[2]: [0.42]    |
| 0.32  | 1           | bucket[1]: [0.32]    |
| 0.23  | 1           | bucket[1]: [0.32, 0.23] |
| 0.52  | 3           | bucket[3]: [0.52]    |
| 0.25  | 1           | bucket[1]: [0.32, 0.23, 0.25] |
| 0.47  | 2           | bucket[2]: [0.42, 0.47] |

**Sort each bucket:**
- bucket[1]: `[0.23, 0.25, 0.32]`
- bucket[2]: `[0.42, 0.47]`
- bucket[3]: `[0.52]`

**Concatenate:** `[0.23, 0.25, 0.32, 0.42, 0.47, 0.52]` ✅

---

## 🐍 Python Implementation

```python
def bucket_sort(arr):
    """Sort floating-point numbers in [0, 1) using Bucket Sort."""
    if not arr:
        return arr

    n = len(arr)
    buckets = [[] for _ in range(n)]     # n buckets for n elements

    # Distribute elements into buckets based on value
    for num in arr:
        index = int(num * n)             # Map [0,1) value to bucket index
        index = min(index, n - 1)        # Guard against edge case num = 1.0
        buckets[index].append(num)

    # Sort each bucket individually (insertion sort is efficient for small lists)
    for bucket in buckets:
        bucket.sort()                    # Python's built-in sort (TimSort)

    # Concatenate all buckets in order
    return [num for bucket in buckets for num in bucket]


def bucket_sort_integers(arr):
    """Sort integers using Bucket Sort by normalizing to [0,1)."""
    if not arr:
        return arr
    min_val, max_val = min(arr), max(arr)
    if min_val == max_val:
        return arr

    n = len(arr)
    buckets = [[] for _ in range(n)]
    for num in arr:
        # Normalize: map [min_val, max_val] to [0, 1)
        normalized = (num - min_val) / (max_val - min_val + 1)
        index = int(normalized * n)
        buckets[index].append(num)

    result = []
    for bucket in buckets:
        bucket.sort()
        result.extend(bucket)
    return result


# Example
arr = [0.42, 0.32, 0.23, 0.52, 0.25, 0.47, 0.51]
print("Sorted:", bucket_sort(arr))
# [0.23, 0.25, 0.32, 0.42, 0.47, 0.51, 0.52]
```

---

## 🎯 Recognize This Problem When...

- Input consists of **floating-point numbers** in a known range (e.g., `[0, 1)`).
- The input is **uniformly distributed** over its range.
- You want to **beat O(n log n)** for the right kind of data.
- Elements can be easily mapped to a bucket index by a simple calculation.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation                                      | Verdict                                    |
|------------------------------------------------|--------------------------------------------|
| Floating-point values in [0, 1) — uniformly distributed | ✅ Near O(n) in practice |
| Large uniform integer datasets                 | ✅ With normalization, works well          |
| Input is NOT uniformly distributed             | ❌ Many elements cluster in same bucket → O(n²) |
| Input range is unknown                         | ❌ Hard to size buckets correctly          |
| Integer data with small range                  | ❌ Counting Sort is simpler and guaranteed |

---

## 🔗 Related Algorithms

| Algorithm                          | How it relates                                              |
|------------------------------------|-------------------------------------------------------------|
| [Counting Sort](CountingSort.md)   | Similar idea but counts exact values instead of ranges      |
| [Radix Sort](RadixSort.md)         | Also non-comparison; works digit by digit                   |
| [Insertion Sort](InsertionSort.md) | Typically used within each bucket (small, nearly-sorted)    |

---

## 📝 Practice Problems

| Problem                              | Platform   | Difficulty |
|-------------------------------------|------------|------------|
| Maximum Gap                         | [LeetCode 164](https://leetcode.com/problems/maximum-gap/) | 🔴 Hard |
| Sort an Array                       | [LeetCode 912](https://leetcode.com/problems/sort-an-array/) | 🟡 Medium |
| Contains Duplicate III              | [LeetCode 220](https://leetcode.com/problems/contains-duplicate-iii/) | 🔴 Hard |
