### **Selection Sort 🔍**
- **Time Complexity:**
  - **Best Case:** O(n²)
  - **Average Case:** O(n²)
  - **Worst Case:** O(n²)
- **Space Complexity:** O(1) *(in-place sorting)*
- **Stable:** ❌ No (can be made stable with modifications)
- **Adaptive:** ❌ No (always does the same number of comparisons)

---

#### **How It Works**
Selection Sort divides the array into two parts:
1. The **sorted** part at the beginning.
2. The **unsorted** part at the end.

It repeatedly:
- **Finds the minimum** (or maximum) element from the unsorted part.
- **Swaps** it with the first unsorted element.

---

#### **Python Implementation**
```python
def selection_sort(arr):
    n = len(arr)
    for i in range(n):
        min_index = i
        for j in range(i + 1, n):
            if arr[j] < arr[min_index]:
                min_index = j
        arr[i], arr[min_index] = arr[min_index], arr[i]
    return arr

# Example
arr = [64, 25, 12, 22, 11]
print("Sorted array:", selection_sort(arr))
```

---

#### When to Use
✅ Easy to implement  
✅ Works well for **small** datasets  
🚫 Not efficient for large arrays  
🚫 Not adaptive — does the same number of comparisons regardless of input

---
