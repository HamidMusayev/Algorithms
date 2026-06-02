# Line Intersection 📐

---

## 🧠 Intuition

Two line segments may or may not cross. The key tool is the **cross product** / **orientation test**: given three points O, A, B, determine whether the turn from OA to OB is clockwise, counter-clockwise, or collinear (straight).

Two segments AB and CD intersect if and only if:
- A and B are on **opposite sides** of line CD, AND
- C and D are on **opposite sides** of line AB.

Think of it as: "Can you travel from A to B and cross the CD line? And can you travel from C to D and cross the AB line?"

**Mental model:** "Opposite orientations ↔ the segments cross."

---

## 📊 Complexity

| Operation | Time |
|-----------|------|
| Single pair intersection test | O(1) |
| n segments, all intersections (Bentley-Ottmann) | O((n + k) log n) |

---

## ⚙️ How It Works

**Orientation of three points O, A, B:**
```
cross = (A.x - O.x) × (B.y - O.y) - (A.y - O.y) × (B.x - O.x)
cross > 0 → Counter-clockwise (CCW)
cross < 0 → Clockwise (CW)
cross = 0 → Collinear
```

**Intersection test for segments P1P2 and P3P4:**
1. Compute orientations: `o1=orient(P1,P2,P3)`, `o2=orient(P1,P2,P4)`, `o3=orient(P3,P4,P1)`, `o4=orient(P3,P4,P2)`.
2. **General case:** `o1 ≠ o2` AND `o3 ≠ o4` → segments intersect.
3. **Collinear special cases:** if orientation is 0, check if point lies on the segment.

---

## 🔢 Step-by-Step Trace

Segment 1: P1=(1,1) to P2=(10,1) (horizontal)
Segment 2: P3=(1,2) to P4=(10,2) (parallel horizontal)

- `o1 = orient(P1,P2,P3)`: P3=(1,2) is above the line → CCW (+1)
- `o2 = orient(P1,P2,P4)`: P4=(10,2) is above the line → CCW (+1)
- `o1 == o2` → P3 and P4 are on the **same side** → **NO intersection** ✅

Segment 1: P1=(0,0) to P2=(10,10) (diagonal)
Segment 2: P3=(0,10) to P4=(10,0) (other diagonal)

- `o1 = orient(P1,P2,P3)`: P3=(0,10) above the 45° line → CCW (+1)
- `o2 = orient(P1,P2,P4)`: P4=(10,0) below → CW (-1)
- `o3 = orient(P3,P4,P1)`: P1=(0,0) below P3P4 → CW (-1)
- `o4 = orient(P3,P4,P2)`: P2=(10,10) above P3P4 → CCW (+1)
- `o1 ≠ o2` AND `o3 ≠ o4` → **INTERSECT at (5,5)** ✅

---

## 🐍 Python Implementation

```python
def orient(p, q, r):
    """
    Compute orientation of triplet (p, q, r).
    Returns: 1 (CCW), -1 (CW), 0 (collinear)
    """
    val = (q[0] - p[0]) * (r[1] - p[1]) - (q[1] - p[1]) * (r[0] - p[0])
    if val > 0: return 1    # Counter-clockwise
    if val < 0: return -1   # Clockwise
    return 0                # Collinear


def on_segment(p, q, r):
    """Check if point q lies on segment pr (assumes p, q, r are collinear)."""
    return (min(p[0], r[0]) <= q[0] <= max(p[0], r[0]) and
            min(p[1], r[1]) <= q[1] <= max(p[1], r[1]))


def segments_intersect(p1, p2, p3, p4):
    """
    Check if segment P1P2 intersects segment P3P4.
    Handles collinear edge cases.
    """
    o1 = orient(p1, p2, p3)
    o2 = orient(p1, p2, p4)
    o3 = orient(p3, p4, p1)
    o4 = orient(p3, p4, p2)

    # General case: segments on opposite sides of each other
    if o1 != o2 and o3 != o4:
        return True

    # Collinear cases: check if endpoint lies on the other segment
    if o1 == 0 and on_segment(p1, p3, p2): return True
    if o2 == 0 and on_segment(p1, p4, p2): return True
    if o3 == 0 and on_segment(p3, p1, p4): return True
    if o4 == 0 and on_segment(p3, p2, p4): return True

    return False


def intersection_point(p1, p2, p3, p4):
    """
    Find the actual intersection point of segments P1P2 and P3P4.
    Returns None if they don't intersect or are parallel.
    """
    if not segments_intersect(p1, p2, p3, p4):
        return None

    # Parametric form: P1 + t*(P2-P1) = P3 + s*(P4-P3)
    dx1 = p2[0] - p1[0]; dy1 = p2[1] - p1[1]
    dx2 = p4[0] - p3[0]; dy2 = p4[1] - p3[1]
    denom = dx1 * dy2 - dy1 * dx2

    if denom == 0:
        return None  # Parallel (or collinear with infinite intersections)

    t = ((p3[0] - p1[0]) * dy2 - (p3[1] - p1[1]) * dx2) / denom

    return (p1[0] + t * dx1, p1[1] + t * dy1)


# Examples
print(segments_intersect((1,1),(10,1),(1,2),(10,2)))    # False (parallel)
print(segments_intersect((0,0),(10,10),(0,10),(10,0)))   # True
print(intersection_point((0,0),(10,10),(0,10),(10,0)))   # (5.0, 5.0)
```

---

## 🎯 Recognize This Problem When...

- You need to check if two line segments **cross each other**.
- Problems involving **convex hull** (checking left/right turns uses orientation).
- Polygon collision detection, ray casting.
- Computing the **sweep line** event for segment intersections.
- Keywords: "do these segments cross?", "does this point lie on this segment?".

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Single pair of segments: do they cross? | ✅ O(1) orientation test |
| Polygon inside/outside point test | ✅ Ray casting uses intersection |
| n segments: find all crossings | ❌ Use Bentley-Ottmann sweep line |
| Lines (infinite), not segments | ✅ Skip the collinear edge cases |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Convex Hull](ConvexHull.md) | Uses orientation/cross product for turn detection |
| [Sweep Line](SweepLine.md) | Bentley-Ottmann uses intersection test as a subroutine |
| [Closest Pair of Points](ClosestPairOfPoints.md) | Also fundamental computational geometry |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Erect the Fence](https://leetcode.com/problems/erect-the-fence/) | LeetCode | 🔴 Hard |
| [Intersection of Two Line Segments](https://leetcode.com/problems/intersection-of-two-linked-lists/) | LeetCode | 🟢 Easy (different) |
| [Segment Intersection](https://www.hackerrank.com/challenges/the-grid-search/problem) | HackerRank | 🟡 Medium |
