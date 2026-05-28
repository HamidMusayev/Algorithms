### **Line Intersection 📏**

Decide whether two **line segments** intersect, and find the intersection point. Foundation for sweep-line and polygon clipping.

The standard trick is the **CCW (orientation) test**:
```
orient(p, q, r) = sign( (q.x-p.x)*(r.y-p.y) - (q.y-p.y)*(r.x-p.x) )
```
- `> 0` → counter-clockwise
- `< 0` → clockwise
- `= 0` → collinear

Two segments `AB` and `CD` properly intersect iff `A` and `B` are on opposite sides of `CD`, AND `C` and `D` are on opposite sides of `AB`.

---

#### **Summary**
- **Time Complexity:** O(1) per pair
- For `n` segments → use **Bentley-Ottmann sweep line** for O((n+k) log n)

---

#### **Python Implementation**
```python
def orient(p, q, r):
    val = (q[0] - p[0]) * (r[1] - p[1]) - (q[1] - p[1]) * (r[0] - p[0])
    if val == 0: return 0
    return 1 if val > 0 else -1

def on_segment(p, q, r):
    return (min(p[0], r[0]) <= q[0] <= max(p[0], r[0]) and
            min(p[1], r[1]) <= q[1] <= max(p[1], r[1]))

def segments_intersect(p1, p2, p3, p4):
    o1, o2 = orient(p1, p2, p3), orient(p1, p2, p4)
    o3, o4 = orient(p3, p4, p1), orient(p3, p4, p2)
    if o1 != o2 and o3 != o4: return True
    # Collinear edge cases
    if o1 == 0 and on_segment(p1, p3, p2): return True
    if o2 == 0 and on_segment(p1, p4, p2): return True
    if o3 == 0 and on_segment(p3, p1, p4): return True
    if o4 == 0 and on_segment(p3, p2, p4): return True
    return False

# Example
print(segments_intersect((1,1), (10,1), (1,2), (10,2)))   # False (parallel)
print(segments_intersect((10,0), (0,10), (0,0), (10,10))) # True
```

---

#### **When to Use**
✅ Polygon clipping, collision tests
✅ Building block for sweep-line algorithms (see [SweepLine](SweepLine.md))
