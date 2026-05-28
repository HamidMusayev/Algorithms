### **Insertion Sort 🔍**
- **Time Complexity:**
  - **Best Case:** O(n) – when array is already sorted
  - **Average Case:** O(n²)
  - **Worst Case:** O(n²)
- **Space Complexity:** O(1) *(in-place sort)*
- **Stable:** ✅ Yes
- **Adaptive:** ✅ Yes – faster for nearly sorted arrays

---

#### **How It Works**
Insertion Sort builds the sorted array **one item at a time**:
1. It starts from the second element.
2. Compares it with elements before it.
3. Inserts it into the correct position in the sorted part.

Think of how you sort playing cards in your hand! 🃏

---

### Python Implementation
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]
        j = i - 1

        # Shift elements of arr[0..i-1] that are > key
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1

        arr[j + 1] = key  # Place key at the right spot
    return arr

### Example
arr = [12, 11, 13, 5, 6]
print("Sorted array:", insertion_sort(arr))
```

---

#### When to Use
✅ Great for **small or nearly sorted datasets**  
✅ Simple and efficient for real-time data input  
🚫 Not suitable for large unsorted data sets  

---

### Comparison with Previous Sorts

| Sort            | Best Case | Average Case | Worst Case | Stable | Adaptive |
|-----------------|-----------|--------------|------------|--------|----------|
| Bubble Sort     | O(n)      | O(n²)        | O(n²)      | ✅ Yes | ✅ Yes   |
| Selection Sort  | O(n²)     | O(n²)        | O(n²)      | ❌ No  | ❌ No    |
| **Insertion Sort** | **O(n)** | **O(n²)**    | **O(n²)**  | ✅ Yes | ✅ Yes   |

---
