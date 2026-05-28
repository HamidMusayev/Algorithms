### **Closest Pair of Points 📏**

Given `n` points in 2D, find the **pair with minimum Euclidean distance**.

- **Brute force:** O(n²)
- **Divide & Conquer:** **O(n log n)**

---

#### **How D&C Works**
1. Sort points by `x`.
2. Recursively find the closest pair in the left half and right half. Let `d = min(dL, dR)`.
3. Check the "strip" of points within `d` of the dividing line; sort those by `y`.
4. For each point in the strip, only the next 7 points can possibly be closer than `d`.

---

#### **Python Implementation**
```python
import math

def closest_pair(points):
    points = sorted(points)
    def dist(p, q): return math.hypot(p[0]-q[0], p[1]-q[1])

    def rec(pts):
        n = len(pts)
        if n <= 3:
            return min((dist(pts[i], pts[j]), pts[i], pts[j])
                       for i in range(n) for j in range(i+1, n))
        mid = n // 2
        midx = pts[mid][0]
        dL = rec(pts[:mid])
        dR = rec(pts[mid:])
        d, p, q = min(dL, dR, key=lambda x: x[0])

        strip = sorted([pt for pt in pts if abs(pt[0] - midx) < d], key=lambda x: x[1])
        for i in range(len(strip)):
            for j in range(i+1, min(i+8, len(strip))):
                nd = dist(strip[i], strip[j])
                if nd < d:
                    d, p, q = nd, strip[i], strip[j]
        return d, p, q

    return rec(points)

# Example
pts = [(2,3), (12,30), (40,50), (5,1), (12,10), (3,4)]
print(closest_pair(pts))   # (1.41..., (2,3), (3,4))
```

---

#### **When to Use**
✅ Computational geometry, clustering, animation
✅ Beats brute O(n²) for large point sets
