### **Greatest Common Divisor (GCD) — Euclidean Algorithm 🔢**

GCD of two numbers `a` and `b` is the largest integer that divides both. Euclid's identity:
```
gcd(a, b) = gcd(b, a mod b)
gcd(a, 0) = a
```

---

#### **Summary**
- **Time Complexity:** O(log min(a, b))
- **Space Complexity:** O(1) iterative, O(log) recursive

---

#### **Python Implementation**
```python
def gcd(a, b):
    while b:
        a, b = b, a % b
    return a

# Recursive
def gcd_rec(a, b):
    return a if b == 0 else gcd_rec(b, a % b)

# Extended Euclidean: returns (g, x, y) with a*x + b*y = g
def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    return g, y1, x1 - (a // b) * y1

# Example
print(gcd(48, 18))             # 6
print(extended_gcd(30, 18))    # (6, -1, 2)   30*-1 + 18*2 = 6
```

---

#### **When to Use**
✅ Building block for nearly all number theory
✅ Simplifying fractions, modular inverses (via extended Euclid)
✅ Computing LCM: `lcm(a,b) = a*b // gcd(a,b)`
