### **Bubble Sort 🔍**  
- **Time Complexity:**  
  - **Best Case:** O(n) *(when array is already sorted – with optimization)*  
  - **Average Case:** O(n²)  
  - **Worst Case:** O(n²)  
- **Space Complexity:** O(1) *(in-place sorting)*  
- **Stable:** ✅ Yes  
- **Adaptive:** ✅ Yes (with optimization)  

---

#### **Explanation**  
Bubble Sort repeatedly steps through the list, compares adjacent items, and **swaps them** if they are in the wrong order. The largest element "bubbles up" to the end in each pass.

---

#### **Python Implementation**
```python
def bubble_sort(arr):
    n = len(arr)
    for i in range(n):
        swapped = False  # Optimization
        for j in range(0, n - i - 1):
            if arr[j] > arr[j + 1]:
                arr[j], arr[j + 1] = arr[j + 1], arr[j]
                swapped = True
        if not swapped:
            break  # Array already sorted
    return arr

# Example
arr = [64, 34, 25, 12, 22, 11, 90]
print("Sorted array:", bubble_sort(arr))
```

---

#### **C# Implementation**
```csharp
using System;

class BubbleSortExample {
    static void BubbleSort(int[] arr) {
        int n = arr.Length;
        for (int i = 0; i < n; i++) {
            bool swapped = false;
            for (int j = 0; j < n - i - 1; j++) {
                if (arr[j] > arr[j + 1]) {
                    // Swap
                    int temp = arr[j];
                    arr[j] = arr[j + 1];
                    arr[j + 1] = temp;
                    swapped = true;
                }
            }
            if (!swapped)
                break; // Already sorted
        }
    }

    static void Main() {
        int[] arr = {64, 34, 25, 12, 22, 11, 90};
        BubbleSort(arr);
        Console.WriteLine("Sorted array: " + string.Join(", ", arr));
    }
}
```

---

#### When to Use
✅ **Simple to understand & implement**  
✅ **Good for educational purposes**  
🚫 **Not efficient for large datasets**  
