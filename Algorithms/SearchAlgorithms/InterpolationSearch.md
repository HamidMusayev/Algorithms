### **Interpolation Search Algorithm** 🔍  
Interpolation Search is an **improved version of Binary Search** that works efficiently when elements in the array are **uniformly distributed**. Instead of always checking the middle element, it **estimates** the position of the target using interpolation formula.

---

### **How Interpolation Search Works**
1. **Estimate** the probable position of the target using the formula:  
`pos=low+(((target - arr[low])×(high−low))/(arr[high]−arr[low]))`
3. If `arr[pos]` is the target, return `pos`.
4. If `arr[pos]` is **less** than the target, search the **right half**.
5. If `arr[pos]` is **greater** than the target, search the **left half**.
6. Repeat until the element is found or search space is exhausted.

---

### **Time & Space Complexity**
| Case        | Time Complexity  | Space Complexity |
|------------|----------------|----------------|
| **Best**   | **O(1)** (If element is found in the first attempt) | **O(1)** (Uses only a few variables) |
| **Worst**  | **O(n)** (If values are not uniformly distributed) | **O(1)** |
| **Average**| **O(log log n)** (Better than Binary Search for uniform distribution) | **O(1)** |

---

### **Interpolation Search Implementation**
Below is a **single implementation** in both **Python and C#**.

---

#### **Python Implementation**
```python
def interpolation_search(arr, target):
    low, high = 0, len(arr) - 1

    while low <= high and arr[low] <= target <= arr[high]:
        # Estimate position using interpolation formula
        pos = low + ((target - arr[low]) * (high - low) // (arr[high] - arr[low]))

        if arr[pos] == target:
            return pos  # Found, return index
        elif arr[pos] < target:
            low = pos + 1  # Search right half
        else:
            high = pos - 1  # Search left half

    return -1  # Not found

# Example Usage
arr = [10, 20, 30, 40, 50, 60, 70, 80, 90]
target = 40

result = interpolation_search(arr, target)
print(f"Element {target} found at index: {result}" if result != -1 else "Element not found")
```
**Example Output:**
```
Element 40 found at index: 3
```

---

#### **C# Implementation**
```csharp
using System;

class InterpolationSearchExample {
    static int InterpolationSearch(int[] arr, int target) {
        int low = 0, high = arr.Length - 1;

        while (low <= high && arr[low] <= target && target <= arr[high]) {
            // Estimate position using interpolation formula
            int pos = low + ((target - arr[low]) * (high - low) / (arr[high] - arr[low]));

            if (arr[pos] == target)
                return pos; // Found, return index
            else if (arr[pos] < target)
                low = pos + 1; // Search right half
            else
                high = pos - 1; // Search left half
        }

        return -1; // Not found
    }

    static void Main() {
        int[] arr = {10, 20, 30, 40, 50, 60, 70, 80, 90};
        int target = 40;

        int result = InterpolationSearch(arr, target);

        if (result != -1)
            Console.WriteLine($"Element {target} found at index: {result}");
        else
            Console.WriteLine("Element not found");
    }
}
```
**Example Output:**
```
Element 40 found at index: 3
```

---

### **When to Use Interpolation Search?**
✅ **Works best on uniformly distributed data** – If data is evenly spaced, it can outperform Binary Search.  
✅ **Faster than Binary Search in some cases** – It achieves **O(log log n)** complexity instead of **O(log n)**.  
✅ **Better for large datasets** – Since it estimates the target's position rather than dividing the list in half.

🚫 **Not ideal for non-uniform data** – Performance can degrade to **O(n)** if values are clustered unevenly.

---
