Absolutely! Let's dig into **Heap Sort**, another powerful comparison-based sorting algorithm. 🏗️

---

### **Heap Sort 🔍**

Heap Sort uses a **binary heap** (usually a **max-heap**) to sort elements. It repeatedly extracts the maximum element from the heap and places it at the end of the array.

---

#### **Summary**

- **Time Complexity:**
  - **Best:** O(n log n)
  - **Average:** O(n log n)
  - **Worst:** O(n log n)
- **Space Complexity:** O(1) *(in-place)*
- **Stable:** ❌ No
- **In-place:** ✅ Yes

---

#### How It Works

1. Build a **max heap** from the array.
2. Swap the first (largest) element with the last.
3. Reduce the heap size and heapify the root again.
4. Repeat until the heap size is 1.

---

#### Python Implementation

```python
def heapify(arr, n, i):
    largest = i  # Assume root is largest
    left = 2 * i + 1
    right = 2 * i + 2

    # Check if left child is larger
    if left < n and arr[left] > arr[largest]:
        largest = left

    # Check if right child is larger
    if right < n and arr[right] > arr[largest]:
        largest = right

    # If largest is not root, swap and continue heapifying
    if largest != i:
        arr[i], arr[largest] = arr[largest], arr[i]
        heapify(arr, n, largest)

def heap_sort(arr):
    n = len(arr)

    # Build max heap
    for i in range(n // 2 - 1, -1, -1):
        heapify(arr, n, i)

    # Extract elements from heap
    for i in range(n - 1, 0, -1):
        arr[0], arr[i] = arr[i], arr[0]  # Swap
        heapify(arr, i, 0)

    return arr

# Example
arr = [12, 11, 13, 5, 6, 7]
print("Sorted array:", heap_sort(arr))
```

---

#### C# Implementation

```csharp
using System;

class HeapSortExample {
    static void Heapify(int[] arr, int n, int i) {
        int largest = i;
        int left = 2 * i + 1;
        int right = 2 * i + 2;

        if (left < n && arr[left] > arr[largest])
            largest = left;

        if (right < n && arr[right] > arr[largest])
            largest = right;

        if (largest != i) {
            int temp = arr[i];
            arr[i] = arr[largest];
            arr[largest] = temp;

            Heapify(arr, n, largest);
        }
    }

    static void HeapSort(int[] arr) {
        int n = arr.Length;

        // Build max heap
        for (int i = n / 2 - 1; i >= 0; i--)
            Heapify(arr, n, i);

        // Extract elements
        for (int i = n - 1; i > 0; i--) {
            int temp = arr[0];
            arr[0] = arr[i];
            arr[i] = temp;

            Heapify(arr, i, 0);
        }
    }

    static void Main() {
        int[] arr = {12, 11, 13, 5, 6, 7};
        HeapSort(arr);
        Console.WriteLine("Sorted array: " + string.Join(", ", arr));
    }
}
```

---

#### When to Use

✅ Great for **large datasets**  
✅ Consistent **O(n log n)** time  
✅ **In-place** sorting  
🚫 **Not stable** (order of equal elements not preserved)  
🚫 Usually slower than Quick Sort in practice due to more operations

---
