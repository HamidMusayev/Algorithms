### **Exponential Search Algorithm** 🔍  

Exponential Search is an **efficient searching algorithm** that works well for **sorted arrays**. It is particularly useful when the target element is **closer to the beginning** of the array.  

---

### **How Exponential Search Works**
1. Start with **index 1**, then keep doubling (2, 4, 8, ...) until you **exceed the target** or reach the end of the array.
2. Perform **Binary Search** in the identified range (between the last valid step and the exceeded step).

---

### **Time & Space Complexity**
| Case        | Time Complexity  | Space Complexity |
|------------|----------------|----------------|
| **Best**   | **O(1)** (If the target is at the first position) | **O(1)** |
| **Worst**  | **O(log n)** (Since it uses Binary Search) | **O(1)** |
| **Average**| **O(log n)** (Faster than Binary Search in some cases) | **O(1)** |

---

#### **Python Implementation**
```python
def binary_search(arr, left, right, target):
    while left <= right:
        mid = left + (right - left) // 2
        if arr[mid] == target:
            return mid
        elif arr[mid] < target:
            left = mid + 1
        else:
            right = mid - 1
    return -1

def exponential_search(arr, target):
    if arr[0] == target:
        return 0

    size = len(arr)
    index = 1

    while index < size and arr[index] <= target:
        index *= 2  # Double the index

    return binary_search(arr, index // 2, min(index, size - 1), target)

# Example Usage
arr = [2, 3, 4, 10, 40, 80, 120]
target = 10

result = exponential_search(arr, target)
print(f"Element {target} found at index: {result}" if result != -1 else "Element not found")
```
**Example Output:**
```
Element 10 found at index: 3
```

---

### **When to Use Exponential Search?**
✅ **Works well for large sorted arrays** – Quickly finds the range before using Binary Search.  
✅ **Better than Binary Search** when the target is near the beginning.  
✅ **Useful when the size of the array is unknown** – Often used in **unbounded lists** or **infinite arrays**.

🚫 **Not ideal for small arrays** – Binary Search alone may be more efficient.  
🚫 **Requires sorted input** – Cannot work on unsorted arrays.

---

### **Comparison with Other Search Algorithms**
| Search Algorithm | Best Case | Worst Case | Space Complexity | Works on Sorted Arrays? |
|-----------------|----------|------------|------------------|-------------------------|
| **Linear Search** | O(1) | O(n) | O(1) | ❌ No |
| **Binary Search** | O(1) | O(log n) | O(1) | ✅ Yes |
| **Jump Search** | O(1) | O(√n) | O(1) | ✅ Yes |
| **Interpolation Search** | O(1) | O(n) | O(1) | ✅ Yes (Uniform Distribution) |
| **Exponential Search** | O(1) | O(log n) | O(1) | ✅ Yes |

---
