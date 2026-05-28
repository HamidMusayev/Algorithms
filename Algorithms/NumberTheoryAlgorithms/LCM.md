### **Least Common Multiple (LCM) 🔢**

The smallest positive integer divisible by both `a` and `b`. Computed via GCD:
```
lcm(a, b) = |a * b| / gcd(a, b)
```

---

#### **Summary**
- **Time Complexity:** O(log min(a, b))  (dominated by gcd)
- **Space Complexity:** O(1)

---

#### **Python Implementation**
```python
from math import gcd

def lcm(a, b):
    return abs(a * b) // gcd(a, b) if a and b else 0

# LCM of a list
from functools import reduce
def lcm_list(nums):
    return reduce(lcm, nums, 1)

# Example
print(lcm(12, 18))          # 36
print(lcm_list([4, 6, 8]))  # 24
```

> Python 3.9+ has `math.lcm` built in.

---

#### **When to Use**
✅ Period / cycle calculations
✅ Fraction arithmetic (common denominator)
✅ Scheduling problems (synchronizing repeating events)
