### **Counting Sort 🔍**

Instead of comparing elements, **Counting Sort** counts the number of occurrences of each value. It works best for **small integer ranges** (e.g., 0 to k).

---

#### **Summary**

- **Time Complexity:**
  - **Best:** O(n + k)
  - **Average:** O(n + k)
  - **Worst:** O(n + k)
    - `n` = number of elements  
    - `k` = range of input values
- **Space Complexity:** O(k) *(for the count array)*
- **Stable:** ✅ Yes (if implemented properly)
- **In-place:** ❌ No

---

#### How It Works

1. Find the **maximum value** `k` in the array.
2. Create a **count array** of size `k+1` and initialize all to 0.
3. Count the frequency of each value.
4. Use the count array to place elements in sorted order.

---

#### Python Implementation

```python
def counting_sort(arr):
    if not arr:
        return arr

    max_val = max(arr)
    count = [0] * (max_val + 1)

    # Count occurrences
    for num in arr:
        count[num] += 1

    # Build the output array
    sorted_arr = []
    for i in range(len(count)):
        sorted_arr.extend([i] * count[i])

    return sorted_arr

# Example
arr = [4, 2, 2, 8, 3, 3, 1]
print("Sorted array:", counting_sort(arr))
```

---

#### When to Use

✅ Best for **large number of items in a small range**  
✅ Extremely fast for **bounded integers**  
🚫 Not suitable for large `k` (range), especially if sparse  
🚫 Doesn’t work for negative numbers unless adjusted

---
