### **Binary Search Algorithm** 🔍  
Binary Search is a **fast searching algorithm** that works on sorted arrays by repeatedly dividing the search range in half.

---

## **How It Works**
1. Find the **middle** element of the sorted array.
2. If the middle element is **equal** to the target, return its index.
3. If the target is **smaller**, search in the **left half**.
4. If the target is **greater**, search in the **right half**.
5. Repeat until the element is found or the search space is empty.

---

## **Time Complexity**
| Case  | Time Complexity |
|--------|----------------|
| Best   | **O(1)** (if the element is found at the first mid) |
| Worst  | **O(log n)** (keeps dividing the array in half) |
| Average | **O(log n)** |

---

## **C# Implementation (Iterative & Recursive)**
### **1. Iterative Approach**
```csharp
using System;

class BinarySearchExample {
    static int BinarySearch(int[] arr, int target) {
        int left = 0, right = arr.Length - 1;

        while (left <= right) {
            int mid = left + (right - left) / 2;

            if (arr[mid] == target) 
                return mid; // Found, return index
            else if (arr[mid] < target)
                left = mid + 1; // Search right half
            else
                right = mid - 1; // Search left half
        }

        return -1; // Not found
    }

    static void Main() {
        int[] arr = { 5, 12, 34, 67, 89 };
        int target = 34;

        int result = BinarySearch(arr, target);
        
        if (result != -1)
            Console.WriteLine($"Element {target} found at index: {result}");
        else
            Console.WriteLine("Element not found");
    }
}
```
**Example Output:**
```
Element 34 found at index: 2
```

---

### **2. Recursive Approach**
```csharp
using System;

class BinarySearchRecursive {
    static int BinarySearch(int[] arr, int left, int right, int target) {
        if (left > right) return -1; // Base case: not found

        int mid = left + (right - left) / 2;

        if (arr[mid] == target) 
            return mid; // Found, return index
        else if (arr[mid] < target)
            return BinarySearch(arr, mid + 1, right, target); // Search right half
        else
            return BinarySearch(arr, left, mid - 1, target); // Search left half
    }

    static void Main() {
        int[] arr = { 3, 15, 27, 49, 58 };
        int target = 49;

        int result = BinarySearch(arr, 0, arr.Length - 1, target);
        
        if (result != -1)
            Console.WriteLine($"Element {target} found at index: {result}");
        else
            Console.WriteLine("Element not found");
    }
}
```
**Example Output:**
```
Element 49 found at index: 3
```

---

## **Python Implementation (Iterative & Recursive)**
### **1. Iterative Approach**
```python
def binary_search(arr, target):
    left, right = 0, len(arr) - 1

    while left <= right:
        mid = left + (right - left) // 2

        if arr[mid] == target:
            return mid  # Found, return index
        elif arr[mid] < target:
            left = mid + 1  # Search right half
        else:
            right = mid - 1  # Search left half

    return -1  # Not found

# Example Usage
arr = [5, 12, 34, 67, 89]
target = 34

result = binary_search(arr, target)
print(f"Element {target} found at index: {result}" if result != -1 else "Element not found")
```
**Example Output:**
```
Element 34 found at index: 2
```

---

### **2. Recursive Approach**
```python
def binary_search_recursive(arr, left, right, target):
    if left > right:
        return -1  # Base case: not found

    mid = left + (right - left) // 2

    if arr[mid] == target:
        return mid  # Found, return index
    elif arr[mid] < target:
        return binary_search_recursive(arr, mid + 1, right, target)  # Search right half
    else:
        return binary_search_recursive(arr, left, mid - 1, target)  # Search left half

# Example Usage
arr = [3, 15, 27, 49, 58]
target = 49

result = binary_search_recursive(arr, 0, len(arr) - 1, target)
print(f"Element {target} found at index: {result}" if result != -1 else "Element not found")
```
**Example Output:**
```
Element 49 found at index: 3
```

---

## **When to Use Binary Search?**
✅ **Works only on sorted arrays** – If the array is not sorted, sort it first.  
✅ **Efficient for large datasets** – O(log n) is much faster than O(n) of linear search.  
✅ **Used in searching problems** – Like finding a specific element in a database or a dictionary.

🚫 **Not ideal for small datasets** – Linear search may be better for very small lists.

---
