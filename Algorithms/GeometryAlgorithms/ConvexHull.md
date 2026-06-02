# Convex Hull 🔷

---

## 🧠 Intuition

Imagine dropping a handful of nails into a wooden board and stretching a rubber band around all of them, then letting go. The rubber band snaps tight, touching only the outermost nails and forming the smallest convex shape that encloses every nail. That shape — defined by those outermost nails — is the **convex hull**.

More precisely: given a set of 2D points, the convex hull is the smallest convex polygon such that every point lies either on the boundary or inside it.

**Mental model:** "Find the outermost shell of a point cloud."

### The Three Classic Variants

| Algorithm | Time | Core Idea |
|-----------|------|-----------|
| **Graham's Scan** | O(n log n) | Sort points by polar angle from the lowest point, then walk CCW discarding right turns |
| **Jarvis March (Gift Wrapping)** | O(n · h) | Start at the leftmost point, always pick the most counterclockwise next point — like wrapping a gift |
| **Andrew's Monotone Chain** | O(n log n) | Sort by (x, y), build a lower hull and upper hull separately by eliminating clockwise turns |

Andrew's Monotone Chain is generally the simplest to implement and is used below.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time (Graham's Scan) | O(n log n) |
| Time (Andrew's Monotone Chain) | O(n log n) |
| Time (Jarvis March) | O(n · h), where h = hull size |
| Space | O(n) — stores the hull points |

---

## ⚙️ How It Works

Andrew's Monotone Chain builds the hull in two passes over the sorted points:

1. **Sort** all points lexicographically by (x, y).
2. **Build the lower hull** — iterate left to right. For each new point, check if adding it makes a left turn or right turn with the last two hull points using the **cross product**. If it's a right turn (or collinear), pop the last hull point because it's "inside" the hull.
3. **Build the upper hull** — iterate right to left using the same logic.
4. **Concatenate** lower and upper hulls (removing the shared endpoints) to get the full convex hull in counter-clockwise (CCW) order.

**Cross product explained in plain language:**
Given three points O, A, B, the cross product `(A - O) × (B - O)` tells you the signed area of the triangle OAB:
- **Positive** → B is to the left of OA (counter-clockwise turn) — keep it
- **Negative** → B is to the right of OA (clockwise turn) — pop A, it's "inside"
- **Zero** → O, A, B are collinear

```
cross(O, A, B) = (A.x - O.x) * (B.y - O.y) - (A.y - O.y) * (B.x - O.x)
```

---

## 🔢 Step-by-Step Trace

Points: `(0,0), (1,0), (2,0), (1,1), (0,2), (2,2)`

After sorting by (x, y): `(0,0), (0,2), (1,0), (1,1), (2,0), (2,2)`

**Building lower hull (left to right):**

```
Start:  []
Add (0,0):  [(0,0)]
Add (0,2):  [(0,0), (0,2)]
Add (1,0):  cross((0,0),(0,2),(1,0)) = (0*(-2)) - (2*1) = -2 < 0  → pop (0,2)
            [(0,0), (1,0)]
Add (1,1):  cross((0,0),(1,0),(1,1)) = (1*1) - (0*1) = 1 > 0  → keep
            [(0,0), (1,0), (1,1)]
            cross((1,0),(1,1),(2,0))... but we continue...
Add (2,0):  cross((1,0),(1,1),(2,0)) = (0*(-1)) - (1*1) = -1 < 0 → pop (1,1)
            cross((0,0),(1,0),(2,0)) = (1*0) - (0*2) = 0 → collinear, pop (1,0)
            [(0,0), (2,0)]
Add (2,2):  [(0,0), (2,0), (2,2)]
```

**Building upper hull (right to left):**
```
Process: (2,2),(2,0),(1,1),(1,0),(0,2),(0,0)
→ upper hull: [(2,2), (1,1), (0,2), (0,0)]
```

**Final hull** (lower[:-1] + upper[:-1]):
`(0,0) → (2,0) → (2,2) → (1,1) → (0,2)` — counter-clockwise

```
(0,2) ---- (2,2)
  \          |
  (1,1)     |
      \      |
(0,0) ---- (2,0)
```

---

## 🐍 Python Implementation

```python
def convex_hull(points):
    # Remove duplicates and sort by (x, y)
    points = sorted(set(map(tuple, points)))
    n = len(points)
    if n <= 1:
        return points

    def cross(O, A, B):
        # Cross product of vectors OA and OB.
        # Positive = CCW turn, Negative = CW turn, Zero = collinear.
        return (A[0] - O[0]) * (B[1] - O[1]) - (A[1] - O[1]) * (B[0] - O[0])

    # Build lower hull: left to right, keep only CCW turns
    lower = []
    for p in points:
        # While the last two points + p make a CW or collinear turn, pop
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    # Build upper hull: right to left, same logic
    upper = []
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    # Remove last point of each half (they are duplicated at junction)
    # Result is in counter-clockwise order
    return lower[:-1] + upper[:-1]


# --- Runnable Example ---
pts = [(0, 0), (1, 1), (2, 2), (3, 1), (4, 0), (2, -1), (1, 0)]
hull = convex_hull(pts)
print("Convex hull (CCW):", hull)
# Expected: [(0, 0), (4, 0), (2, -1)] extended to include corners
# Output: [(0, 0), (2, -1), (4, 0), (3, 1), (2, 2)]

# Verify: interior point (1,0) should NOT be in the hull
print("Interior point (1,0) in hull?", (1, 0) in hull)  # False
```

---

## 🎯 Recognize This Problem When...

- You need the **outermost boundary** of a set of points
- The problem asks to "wrap" or "enclose" a point cloud
- You need to check if a polygon or shape is convex
- The problem involves **collision detection** or **bounding shapes**
- You need to find the **maximum area** or **perimeter** shape from a set of points
- The problem involves **gift wrapping**, fences, or enclosing regions
- Keywords: "smallest enclosing polygon", "extreme points", "convex polygon", "rubber band"

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Finding the bounding shape of a point cloud | ✅ Exactly what convex hull does |
| Collision detection (convex objects) | ✅ Convex hulls make intersection tests fast |
| GIS / geographic boundary computation | ✅ Classic application |
| Computer graphics (frustum culling, shadow volumes) | ✅ Common building block |
| Hull size `h` is much smaller than `n` | ✅ Jarvis March is O(n·h) — may be faster |
| You need a **concave** boundary (following exact shape) | ❌ Use alpha shapes or concave hull algorithms instead |
| Points are already sorted and hull is trivial | ❌ Overhead not worth it for very small n |
| You need the hull in 3D | ❌ 2D algorithms don't generalize — use 3D convex hull (Quickhull 3D, etc.) |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Line Intersection](LineIntersection.md) | Used inside hull algorithms to test turn direction via cross product |
| [Closest Pair of Points](ClosestPairOfPoints.md) | Both are fundamental 2D geometry problems often taught together |
| [Voronoi Diagram](VoronoiDiagram.md) | Convex hull of a point set is the dual of the outermost Delaunay triangulation cells |
| [Sweep Line](SweepLine.md) | Fortune's algorithm (Voronoi) uses the sweep-line paradigm; related mindset |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Erect the Fence](https://leetcode.com/problems/erect-the-fence/) | LeetCode | 🔴 Hard |
| [Max Points on a Line](https://leetcode.com/problems/max-points-on-a-line/) | LeetCode | 🔴 Hard |
| [Minimum Area Rectangle II](https://leetcode.com/problems/minimum-area-rectangle-ii/) | LeetCode | 🟡 Medium |
| [Convex Hull](https://www.hackerrank.com/challenges/convex-hull-fp/problem) | HackerRank | 🟡 Medium |
| [Fence Orthogonality](https://codeforces.com/problemset/problem/598/F) | Codeforces | 🔴 Hard |
