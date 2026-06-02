# Closest Pair of Points 📍

---

## 🧠 Intuition

Given n points on a map, find the two cities closest together. Brute force checks every pair: O(n²) — too slow for large n.

The divide-and-conquer approach: split points into left and right halves, recursively find the closest pair in each half, then check only points near the dividing line (the "strip"). The key insight: within the strip, each point only needs to be compared to the next 7 points (by y-coordinate) due to geometry — so the strip check is O(n).

**Mental model:** Split → solve left half → solve right half → merge by checking a narrow strip around the dividing line.

---

## 📊 Complexity

| Approach | Time |
|----------|------|
| Brute force | O(n²) |
| Divide & Conquer | O(n log n) |
| Space | O(n) |

---

## ⚙️ How It Works

1. **Sort** all points by x-coordinate.
2. **Divide** at the median x-value: left half and right half.
3. **Conquer:** Recursively find `dL` (min distance in left) and `dR` (min distance in right). Let `d = min(dL, dR)`.
4. **Merge:** Check the **strip** — all points within distance `d` of the dividing line:
   - Sort strip points by y-coordinate.
   - For each strip point, compare with the next 7 points (by y). If any is closer than `d`, update `d`.
5. Return `d` and the closest pair.

**Why only 7 points in the strip?** In a `d × 2d` rectangle, you can pack at most 8 points with pairwise distance ≥ d (geometric argument).

---

## 🔢 Step-by-Step Trace

Points: `(0,0), (1,1), (3,4), (5,2), (6,5)`

1. Sort by x: `[(0,0),(1,1),(3,4),(5,2),(6,5)]`, divide at `x=3`.
2. Left: `[(0,0),(1,1),(3,4)]` → closest = `(0,0)-(1,1)`, d=√2 ≈ 1.41
3. Right: `[(3,4),(5,2),(6,5)]` → closest = `(5,2)-(6,5)` d=√10, or `(3,4)-(5,2)` d≈2.83... actually `(5,2)-(6,5)` ≈ 3.16, `(3,4)-(6,5)` ≈ 3.16, `(3,4)-(5,2)` ≈ 2.83. d=2.83
4. `d = min(1.41, 2.83) = 1.41`
5. Strip: points within 1.41 of x=3: `(1,1),(3,4)`. Check: dist ≈ 3.16 > 1.41. No improvement.
6. Answer: `(0,0)-(1,1)`, distance ≈ 1.41 ✅

---

## 🐍 Python Implementation

```python
import math

def dist(p, q):
    """Euclidean distance between two points."""
    return math.hypot(p[0] - q[0], p[1] - q[1])


def brute_force(pts):
    """O(n²) — only for small n (used as base case)."""
    min_d = float('inf')
    pair = (pts[0], pts[1])
    for i in range(len(pts)):
        for j in range(i + 1, len(pts)):
            d = dist(pts[i], pts[j])
            if d < min_d:
                min_d, pair = d, (pts[i], pts[j])
    return min_d, pair


def closest_pair(points):
    """O(n log n) divide-and-conquer closest pair of points."""
    points = sorted(points)          # Sort by x-coordinate once

    def rec(pts):
        n = len(pts)
        if n <= 3:
            return brute_force(pts)  # Base case: brute force for small sets

        mid = n // 2
        mid_x = pts[mid][0]

        d_left, pair_left = rec(pts[:mid])
        d_right, pair_right = rec(pts[mid:])

        # Take the better of left and right
        d = d_left
        best_pair = pair_left
        if d_right < d:
            d, best_pair = d_right, pair_right

        # Check the strip: points within d of the dividing line
        strip = [p for p in pts if abs(p[0] - mid_x) < d]
        strip.sort(key=lambda p: p[1])   # Sort strip by y-coordinate

        # Compare each strip point with next 7 (at most) by y-value
        for i in range(len(strip)):
            for j in range(i + 1, min(i + 8, len(strip))):
                if strip[j][1] - strip[i][1] >= d:
                    break             # No need to check further (sorted by y)
                nd = dist(strip[i], strip[j])
                if nd < d:
                    d, best_pair = nd, (strip[i], strip[j])

        return d, best_pair

    return rec(points)


# Example
pts = [(2, 3), (12, 30), (40, 50), (5, 1), (12, 10), (3, 4)]
d, (p1, p2) = closest_pair(pts)
print(f"Closest pair: {p1} and {p2}")      # (2,3) and (3,4)
print(f"Distance: {d:.4f}")                # 1.4142...
```

---

## 🎯 Recognize This Problem When...

- You need to find the **two nearest elements** in a set of 2D (or higher-dimensional) points.
- Keywords: "closest two cities", "minimum distance between any two points".
- The problem has a large n (> 1000) where O(n²) would TLE.
- Clustering or spatial analysis applications.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Large n (thousands of points) | ✅ O(n log n) — scales well |
| Preprocessing for repeated queries | ✅ Precompute once |
| Small n (< 100 points) | ❌ Brute force O(n²) is simpler |
| Points in 3D or higher | ❌ Algorithm extends but becomes more complex |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Convex Hull](ConvexHull.md) | Also fundamental geometry; different problem |
| [Line Intersection](LineIntersection.md) | Another core computational geometry primitive |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Closest Pair of Points](https://www.hackerrank.com/challenges/closest-numbers/problem) | HackerRank | 🟢 Easy |
| [K Closest Points to Origin](https://leetcode.com/problems/k-closest-points-to-origin/) | LeetCode | 🟡 Medium |
| [Find K Closest Elements](https://leetcode.com/problems/find-k-closest-elements/) | LeetCode | 🟡 Medium |
