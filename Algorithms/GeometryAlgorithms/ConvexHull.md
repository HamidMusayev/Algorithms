### **Convex Hull 📏**

Given a set of points in 2D, the **convex hull** is the smallest convex polygon containing all of them. Think of a rubber band wrapped around all the points.

Common algorithms:
- **Graham's Scan** – O(n log n) via polar sort
- **Jarvis March (Gift Wrapping)** – O(n · h), where `h` = hull size
- **Andrew's Monotone Chain** – O(n log n), simplest to code

---

#### **Summary**
| Algorithm | Time | Notes |
|----------|------|-------|
| Graham's Scan | O(n log n) | sort by polar angle |
| Andrew's Chain | O(n log n) | sort by `(x, y)` |
| Jarvis March | O(n · h) | good when hull is small |

---

#### **Python Implementation (Andrew's Monotone Chain)**
```python
def convex_hull(points):
    points = sorted(set(map(tuple, points)))
    if len(points) <= 1: return points

    def cross(o, a, b):
        return (a[0]-o[0])*(b[1]-o[1]) - (a[1]-o[1])*(b[0]-o[0])

    lower = []
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]   # CCW order

# Example
pts = [(0,0), (1,1), (2,2), (3,1), (4,0), (2,-1), (1,0)]
print(convex_hull(pts))   # [(0,0), (4,0), (3,1), (2,2), (1,1)]
```

---

#### **When to Use**
✅ Computer graphics, collision detection
✅ GIS, robotics, shape analysis
✅ Foundation for many other geometry algorithms
