### **Merge Sort 🔍**
A classic **divide-and-conquer** algorithm that:
1. Divides the array into halves recursively.
2. Sorts each half.
3. Merges the sorted halves back together.

---

#### **Highlights**
- **Time Complexity:**
  - **Best:** O(n log n)
  - **Average:** O(n log n)
  - **Worst:** O(n log n)
- **Space Complexity:** O(n) – due to additional arrays used for merging
- **Stable:** ✅ Yes
- **Adaptive:** ❌ No

---

#### How It Works
Imagine cutting a deck of cards in half, sorting both halves, then perfectly merging them into a sorted deck. ♠️♦️

---

#### Python Implementation
```python
def merge_sort(arr):
    if len(arr) > 1:
        mid = len(arr) // 2  # Find the middle
        left_half = arr[:mid]
        right_half = arr[mid:]

        merge_sort(left_half)
        merge_sort(right_half)

        # Merge the two halves
        i = j = k = 0

        # Copy data to temp arrays
        while i < len(left_half) and j < len(right_half):
            if left_half[i] < right_half[j]:
                arr[k] = left_half[i]
                i += 1
            else:
                arr[k] = right_half[j]
                j += 1
            k += 1

        # Remaining elements
        while i < len(left_half):
            arr[k] = left_half[i]
            i += 1
            k += 1

        while j < len(right_half):
            arr[k] = right_half[j]
            j += 1
            k += 1

    return arr

# Example
arr = [38, 27, 43, 3, 9, 82, 10]
print("Sorted array:", merge_sort(arr))
```

---

#### C# Implementation
```csharp
using System;

class MergeSortExample {
    static void MergeSort(int[] arr, int left, int right) {
        if (left < right) {
            int mid = (left + right) / 2;

            MergeSort(arr, left, mid);
            MergeSort(arr, mid + 1, right);

            Merge(arr, left, mid, right);
        }
    }

    static void Merge(int[] arr, int left, int mid, int right) {
        int n1 = mid - left + 1;
        int n2 = right - mid;

        int[] leftArr = new int[n1];
        int[] rightArr = new int[n2];

        Array.Copy(arr, left, leftArr, 0, n1);
        Array.Copy(arr, mid + 1, rightArr, 0, n2);

        int i = 0, j = 0, k = left;

        while (i < n1 && j < n2) {
            if (leftArr[i] <= rightArr[j])
                arr[k++] = leftArr[i++];
            else
                arr[k++] = rightArr[j++];
        }

        while (i < n1) arr[k++] = leftArr[i++];
        while (j < n2) arr[k++] = rightArr[j++];
    }

    static void Main() {
        int[] arr = {38, 27, 43, 3, 9, 82, 10};
        MergeSort(arr, 0, arr.Length - 1);
        Console.WriteLine("Sorted array: " + string.Join(", ", arr));
    }
}
```

---

#### When to Use
✅ Excellent for large datasets  
✅ Guaranteed **O(n log n)** time regardless of data order  
✅ Stable and predictable  
🚫 Not in-place (uses extra memory)  
🚫 Slower than quicksort in practice for small arrays

---
