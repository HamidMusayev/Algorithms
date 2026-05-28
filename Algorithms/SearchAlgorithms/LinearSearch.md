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
| Case  | Time Complexity |
|--------|----------------|
| Best   | **O(1)** (if the element is at the beginning) |
| Worst  | **O(n)** (if the element is at the end or not present) |
| Average | **O(n)** |

### **Space Complexity**
- **O(1)** (Only a few extra variables are used)

---

### **Python Implementation**

#### **1. Iterative Approach**
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

#### **2. Recursive Approach**
```python
def linear_search_recursive(arr, target, index=0):
    if index >= len(arr):
        return -1  # Base case: not found
    if arr[index] == target:
        return index  # Found
    return linear_search_recursive(arr, target, index + 1)

# Example Usage
arr = [10, 20, 30, 40, 50]
print(linear_search_recursive(arr, 40))  # 3
```

---

### **When to Use Each Approach?**
- ✅ **Iterative** – Simple and efficient for most cases.
- ✅ **Recursive** – Cleaner for functional style, but has extra call overhead.

---

### **When to Use Linear Search?**
✅ **Small datasets** – Works well when the list has a small number of elements.  
✅ **Unsorted lists** – Works on both sorted and unsorted lists.  
✅ **No extra memory required** – Unlike binary search, it doesn’t need a sorted array.

🚫 **Not suitable for large datasets** – Inefficient compared to binary search for large lists.
