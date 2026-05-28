### **Shell Sort 🔍**

Shell Sort is a generalization of **Insertion Sort** that allows the exchange of items that are far apart by sorting elements at a specific **gap**, gradually reducing the gap to 1.

---

#### **Summary**
- **Time Complexity:**
  - **Best:** O(n log n)
  - **Average:** depends on gap sequence (≈ O(n^(3/2)) or O(n log² n))
  - **Worst:** O(n²) (with simple `n/2` gap)
- **Space Complexity:** O(1) *(in-place)*
- **Stable:** ❌ No
- **In-place:** ✅ Yes

---

#### **How It Works**
1. Choose a starting gap (commonly `n // 2`).
2. Perform a **gapped insertion sort** for that gap.
3. Halve the gap and repeat until gap = 1 (final insertion sort pass).

---

#### **Python Implementation**
```python
def shell_sort(arr):
    n = len(arr)
    gap = n // 2

    while gap > 0:
        # Gapped insertion sort
        for i in range(gap, n):
            temp = arr[i]
            j = i
            while j >= gap and arr[j - gap] > temp:
                arr[j] = arr[j - gap]
                j -= gap
            arr[j] = temp
        gap //= 2

    return arr

# Example
arr = [12, 34, 54, 2, 3]
print("Sorted array:", shell_sort(arr))
```

---

#### **When to Use**
✅ Faster than Insertion/Bubble Sort on medium-sized arrays
✅ In-place, no recursion overhead
🚫 Not stable
🚫 Performance highly depends on gap sequence
