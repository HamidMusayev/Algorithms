### **Longest Increasing Subsequence (LIS) 📈**

Given an array, find the length of the **longest strictly increasing subsequence** (not necessarily contiguous).

Example: `[10, 9, 2, 5, 3, 7, 101, 18]` → LIS length is `4` (e.g., `[2,3,7,18]`).

---

#### **Summary**
| Approach | Time | Space |
|----------|------|-------|
| DP (n²) | O(n²) | O(n) |
| Binary search + patience sort | **O(n log n)** | O(n) |

---

#### **Python Implementations**

**1. O(n²) DP**
```python
def lis_n2(arr):
    n = len(arr)
    dp = [1] * n
    for i in range(1, n):
        for j in range(i):
            if arr[j] < arr[i]:
                dp[i] = max(dp[i], dp[j] + 1)
    return max(dp) if dp else 0
```

**2. O(n log n) with `bisect`**
```python
from bisect import bisect_left

def lis(arr):
    tails = []
    for x in arr:
        idx = bisect_left(tails, x)
        if idx == len(tails):
            tails.append(x)
        else:
            tails[idx] = x
    return len(tails)

# Example
print(lis([10, 9, 2, 5, 3, 7, 101, 18]))   # 4
```

> Note: `tails` is **not** an actual LIS — only its length is correct. To reconstruct the LIS, track predecessor indices.

---

#### **When to Use**
✅ Stock prediction-style problems
✅ Box stacking, building stacks, longest chain
✅ Foundation for box-stacking and 2D LIS variants
