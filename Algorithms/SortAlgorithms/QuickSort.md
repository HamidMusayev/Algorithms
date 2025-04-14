### **Quick Sort 🔍**

Quick Sort is a **divide-and-conquer** algorithm like Merge Sort, but it works **in-place**, making it faster in practice for most datasets.

---

#### **Summary**

- **Time Complexity:**
  - **Best:** O(n log n)
  - **Average:** O(n log n)
  - **Worst:** O(n²) — happens when pivot selection is poor (e.g., already sorted array with bad pivot)
- **Space Complexity:** O(log n) on average (due to recursion stack)
- **Stable:** ❌ No
- **In-place:** ✅ Yes (no extra array used)

---

#### **How It Works**

1. Choose a **pivot** element.
2. **Partition** the array so that:
   - Elements less than pivot go to the left.
   - Elements greater go to the right.
3. **Recursively** apply the same logic to left and right parts.

---

#### Python Implementation

```python
def quick_sort(arr):
    if len(arr) <= 1:
        return arr

    pivot = arr[len(arr) // 2]  # You can experiment with pivot selection
    left = [x for x in arr if x < pivot]
    middle = [x for x in arr if x == pivot]  # for duplicates
    right = [x for x in arr if x > pivot]

    return quick_sort(left) + middle + quick_sort(right)

# Example
arr = [10, 7, 8, 9, 1, 5]
print("Sorted array:", quick_sort(arr))
```

---

#### C# Implementation

```csharp
using System;

class QuickSortExample {
    static void QuickSort(int[] arr, int low, int high) {
        if (low < high) {
            int pi = Partition(arr, low, high);

            QuickSort(arr, low, pi - 1);
            QuickSort(arr, pi + 1, high);
        }
    }

    static int Partition(int[] arr, int low, int high) {
        int pivot = arr[high]; // using last element as pivot
        int i = (low - 1);

        for (int j = low; j < high; j++) {
            if (arr[j] < pivot) {
                i++;
                (arr[i], arr[j]) = (arr[j], arr[i]); // Swap
            }
        }

        (arr[i + 1], arr[high]) = (arr[high], arr[i + 1]); // Final pivot swap
        return i + 1;
    }

    static void Main() {
        int[] arr = {10, 7, 8, 9, 1, 5};
        QuickSort(arr, 0, arr.Length - 1);
        Console.WriteLine("Sorted array: " + string.Join(", ", arr));
    }
}
```

---

#### When to Use

✅ Best choice for **large, unsorted datasets**  
✅ **In-place & fast**  
🚫 Can degrade to O(n²) if pivot is poorly chosen  
✅ Performant in practice with good pivot strategy (e.g., median-of-three)

---
