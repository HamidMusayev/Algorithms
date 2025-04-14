### **Radix Sort 🔍**

Radix Sort processes **each digit (or character)** of the numbers, one digit at a time, using a **stable sort** like **Counting Sort** as a subroutine.

---

#### **Summary**

- **Time Complexity:**
  - **Best / Average / Worst:** O(n × k)
    - `n` = number of elements
    - `k` = number of digits in the maximum number
- **Space Complexity:** O(n + k)
- **Stable:** ✅ Yes
- **In-place:** ❌ No (due to auxiliary arrays)

---

#### How It Works

1. Find the **maximum number** to determine the number of digits.
2. Sort the array **digit by digit**, starting from the **least significant digit (LSD)** to the most significant.
3. Use **Counting Sort** at each digit level to ensure stability.

---

#### Python Implementation

```python
def counting_sort_for_radix(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10  # Digits 0 to 9

    # Count occurrences
    for i in range(n):
        index = (arr[i] // exp) % 10
        count[index] += 1

    # Cumulative count
    for i in range(1, 10):
        count[i] += count[i - 1]

    # Build output array (stable sort)
    i = n - 1
    while i >= 0:
        index = (arr[i] // exp) % 10
        output[count[index] - 1] = arr[i]
        count[index] -= 1
        i -= 1

    # Copy to original array
    for i in range(n):
        arr[i] = output[i]

def radix_sort(arr):
    max_val = max(arr)

    exp = 1
    while max_val // exp > 0:
        counting_sort_for_radix(arr, exp)
        exp *= 10

    return arr

# Example
arr = [170, 45, 75, 90, 802, 24, 2, 66]
print("Sorted array:", radix_sort(arr))
```

---

#### C# Implementation

```csharp
using System;

class RadixSortExample {
    static int GetMax(int[] arr) {
        int max = arr[0];
        foreach (int num in arr)
            if (num > max) max = num;
        return max;
    }

    static void CountingSort(int[] arr, int exp) {
        int n = arr.Length;
        int[] output = new int[n];
        int[] count = new int[10];

        // Count occurrences
        for (int i = 0; i < n; i++)
            count[(arr[i] / exp) % 10]++;

        // Cumulative count
        for (int i = 1; i < 10; i++)
            count[i] += count[i - 1];

        // Build output array
        for (int i = n - 1; i >= 0; i--) {
            int idx = (arr[i] / exp) % 10;
            output[count[idx] - 1] = arr[i];
            count[idx]--;
        }

        // Copy to original array
        for (int i = 0; i < n; i++)
            arr[i] = output[i];
    }

    static void RadixSort(int[] arr) {
        int max = GetMax(arr);

        for (int exp = 1; max / exp > 0; exp *= 10)
            CountingSort(arr, exp);
    }

    static void Main() {
        int[] arr = {170, 45, 75, 90, 802, 24, 2, 66};
        RadixSort(arr);
        Console.WriteLine("Sorted array: " + string.Join(", ", arr));
    }
}
```

---

#### When to Use

✅ Perfect for sorting **large lists of integers**  
✅ Avoids comparison limits (no O(n log n) barrier)  
🚫 Doesn’t handle **negative numbers** directly  
🚫 Not ideal for **floating-point numbers** or very large `k` (digit count)

---
