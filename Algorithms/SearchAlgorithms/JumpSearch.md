### **Jump Search Algorithm** 🔍  
Jump Search is an **efficient searching algorithm** that works on **sorted arrays** by jumping ahead in fixed steps instead of checking elements one by one.

---

## **How Jump Search Works**
1. **Jump in fixed steps** (usually √n) through the array.
2. If the **current block** contains the target, perform **Linear Search** within the block.
3. If not found, move to the **next block** and repeat until the target is found or the search space is exhausted.

---

## **Time & Space Complexity**
| Case        | Time Complexity  | Space Complexity |
|------------|----------------|----------------|
| **Best**   | **O(1)** (if the element is at the first jump) | **O(1)** (uses only a few extra variables) |
| **Worst**  | **O(√n)** (needs to check multiple blocks) | **O(1)** |
| **Average**| **O(√n)** (since it jumps then performs a linear search) | **O(1)** |

---

## **Jump Search Implementation**
Here’s a single implementation in **Python and C#**.

---

### **Python Implementation**
```python
import math

def jump_search(arr, target):
    n = len(arr)
    step = int(math.sqrt(n))  # Step size
    prev = 0

    while prev < n and arr[min(step, n) - 1] < target:
        prev = step  # Move to next block
        step += int(math.sqrt(n))
        if prev >= n:
            return -1  # Not found

    # Perform Linear Search within the block
    for i in range(prev, min(step, n)):
        if arr[i] == target:
            return i  # Found, return index
    return -1  # Not found

# Example Usage
arr = [1, 3, 5, 7, 9, 11, 13, 15]
target = 9

result = jump_search(arr, target)
print(f"Element {target} found at index: {result}" if result != -1 else "Element not found")
```
**Example Output:**
```
Element 9 found at index: 4
```

---

### **C# Implementation**
```csharp
using System;

class JumpSearchExample {
    static int JumpSearch(int[] arr, int target) {
        int n = arr.Length;
        int step = (int)Math.Floor(Math.Sqrt(n));
        int prev = 0;

        while (prev < n && arr[Math.Min(step, n) - 1] < target) {
            prev = step;
            step += (int)Math.Floor(Math.Sqrt(n));
            if (prev >= n)
                return -1;  // Not found
        }

        // Perform Linear Search within the block
        for (int i = prev; i < Math.Min(step, n); i++) {
            if (arr[i] == target)
                return i;  // Found, return index
        }
        return -1;  // Not found
    }

    static void Main() {
        int[] arr = {1, 3, 5, 7, 9, 11, 13, 15};
        int target = 9;

        int result = JumpSearch(arr, target);

        if (result != -1)
            Console.WriteLine($"Element {target} found at index: {result}");
        else
            Console.WriteLine("Element not found");
    }
}
```
**Example Output:**
```
Element 9 found at index: 4
```

---

### **When to Use Jump Search?**
✅ **Works on sorted arrays** – Just like **Binary Search**, but uses less memory.  
✅ **Efficient for large datasets** – Performs O(√n) comparisons instead of O(n).  
✅ **Better than Linear Search** – Jumps over elements, reducing comparisons.

🚫 **Not the best for very small arrays** – Linear Search may be faster.

---
