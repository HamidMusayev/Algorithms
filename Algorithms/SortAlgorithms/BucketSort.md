### **Bucket Sort 🔍**

Bucket Sort distributes elements into a number of **buckets**, sorts each bucket individually (often with Insertion Sort), and then concatenates them.

---

#### **Summary**
- **Time Complexity:**
  - **Best / Average:** O(n + k)
  - **Worst:** O(n²) – when all elements fall in one bucket
- **Space Complexity:** O(n + k)
- **Stable:** ✅ Yes (if inner sort is stable)
- **In-place:** ❌ No

---

#### **How It Works**
1. Create `k` empty buckets.
2. Place each element into the bucket based on its value.
3. Sort each bucket (e.g., with Insertion Sort).
4. Concatenate buckets in order.

Works best when input is **uniformly distributed** over a known range (e.g., floats in `[0, 1)`).

---

#### **Python Implementation**
```python
def bucket_sort(arr):
    if not arr:
        return arr

    n = len(arr)
    max_val = max(arr)
    buckets = [[] for _ in range(n)]

    # Distribute elements into buckets
    for num in arr:
        index = int(n * num / (max_val + 1))
        buckets[index].append(num)

    # Sort each bucket and concatenate
    sorted_arr = []
    for bucket in buckets:
        sorted_arr.extend(sorted(bucket))
    return sorted_arr

# Example
arr = [0.42, 0.32, 0.23, 0.52, 0.25, 0.47, 0.51]
print("Sorted array:", bucket_sort(arr))
```

---

#### **When to Use**
✅ Great when input is **uniformly distributed**
✅ Can beat O(n log n) on the right data
🚫 Bad when many values cluster into one bucket
🚫 Requires knowledge of the input range
