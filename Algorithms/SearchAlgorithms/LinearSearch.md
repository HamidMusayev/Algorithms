### **Linear Search Algorithm** 🔍
Linear Search is the simplest searching algorithm that checks every element in a list one by one until the target value is found.

---

### **How It Works**
1. Start from the first element.
2. Compare it with the target value.
3. If found, return the index.
4. If not, move to the next element.
5. If the end of the list is reached without finding the element, return `-1` (not found).

---

### **Time Complexity**
- **Best Case:** O(1) (if the element is at the beginning)
- **Worst Case:** O(n) (if the element is at the end or not present)
- **Average Case:** O(n)

### **Space Complexity**
- **O(1)** (Only a few extra variables are used)

---

### **Implementation**

#### **Python Implementation**
```python
def linear_search(arr, target):
    for i in range(len(arr)):
        if arr[i] == target:
            return i  # Found, return index
    return -1  # Not found

# Example Usage
arr = [10, 20, 30, 40, 50]
target = 30
result = linear_search(arr, target)
print("Element found at index:", result) if result != -1 else print("Element not found")
```

#### **C Implementation**
```c
#include <stdio.h>

int linearSearch(int arr[], int size, int target) {
    for (int i = 0; i < size; i++) {
        if (arr[i] == target)
            return i;
    }
    return -1;
}

int main() {
    int arr[] = {10, 20, 30, 40, 50};
    int target = 30;
    int size = sizeof(arr) / sizeof(arr[0]);
    int result = linearSearch(arr, size, target);

    if (result != -1)
        printf("Element found at index: %d\n", result);
    else
        printf("Element not found\n");

    return 0;
}
```

---

#### **C# Implementation**

##### **1. Iterative Approach (Using a Loop)**
```csharp
using System;

class LinearSearchExample {
    static int LinearSearch(int[] arr, int target) {
        for (int i = 0; i < arr.Length; i++) {
            if (arr[i] == target)
                return i; // Found, return index
        }
        return -1; // Not found
    }

    static void Main() {
        int[] arr = { 10, 20, 30, 40, 50 };
        int target = 30;
        
        int result = LinearSearch(arr, target);
        
        if (result != -1)
            Console.WriteLine("Element found at index: " + result);
        else
            Console.WriteLine("Element not found");
    }
}
```
**Output:**
```
Element found at index: 2
```

---

#### **2. Recursive Approach**
```csharp
using System;

class LinearSearchRecursive {
    static int LinearSearch(int[] arr, int target, int index) {
        if (index >= arr.Length) return -1; // Base case: not found
        if (arr[index] == target) return index; // Found, return index
        return LinearSearch(arr, target, index + 1); // Recursive call
    }

    static void Main() {
        int[] arr = { 10, 20, 30, 40, 50 };
        int target = 40;

        int result = LinearSearch(arr, target, 0);
        
        if (result != -1)
            Console.WriteLine("Element found at index: " + result);
        else
            Console.WriteLine("Element not found");
    }
}
```
**Output:**
```
Element found at index: 3
```

---

### **When to Use Each Approach?**
- ✅ **Iterative Approach** – Simple and efficient for most cases.
- ✅ **Recursive Approach** – Useful for functional programming but has extra function call overhead.

---

### **When to Use Linear Search?**
✅ **Small datasets** – Works well when the list has a small number of elements.  
✅ **Unsorted lists** – Works on both sorted and unsorted lists.  
✅ **No extra memory required** – Unlike binary search, it doesn’t need a sorted array.

🚫 **Not suitable for large datasets** – Inefficient compared to binary search for large lists.
