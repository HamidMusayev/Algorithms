# Voronoi Diagram 🗺️

---

## 🧠 Intuition

Imagine dropping colored dye from several sources in a swimming pool. Each dye spreads uniformly; the boundary where two colors meet is equidistant from both sources. The Voronoi diagram is exactly these boundaries: it partitions the plane into **regions**, where each region contains all points closest to one specific "seed" point (site).

Real-world example: given hospital locations, the Voronoi region of each hospital is the area where it's the nearest hospital.

**Mental model:** "For each site, draw the region of all points closer to that site than to any other."

---

## 📊 Complexity

| Approach | Time | Notes |
|----------|------|-------|
| Naive (all-pairs) | O(n² log n) | Simple but slow |
| Fortune's sweep line | O(n log n) | Optimal |
| scipy (Fortune's) | O(n log n) | Best for practical use |
| Space | O(n) | V+E = O(n) by Euler's formula |

---

## ⚙️ How It Works

**Fortune's Algorithm (sweep line):**
1. Maintain a **sweep line** moving left to right.
2. Maintain a **beach line**: a set of parabolic arcs (each arc traces points equidistant from a site and the sweep line).
3. Two types of events:
   - **Site event:** a new site is encountered → insert a new parabola arc into the beach line.
   - **Circle event:** three arcs converge → a Voronoi vertex is created → remove middle arc.
4. Process all events to build the full Voronoi diagram.

> Fortune's algorithm is non-trivial to implement from scratch. In practice, use **scipy** or a geometry library.

---

## 🔢 Step-by-Step Trace

4 sites: `(0,0), (4,0), (2,3), (2,-3)`

The Voronoi regions approximately:
```
     (2,3)
    /  |  \
(0,0)  |  (4,0)
    \  |  /
     (2,-3)
```

- All points above the line y=0 between x=0 and x=4 are closest to (2,3).
- The horizontal strip near y=0 splits between (0,0) and (4,0).
- Each region is a convex polygon.

---

## 🐍 Python Implementation

```python
import numpy as np
from scipy.spatial import Voronoi, voronoi_plot_2d
import matplotlib.pyplot as plt

def compute_voronoi(sites):
    """
    Compute Voronoi diagram using scipy (Fortune's algorithm internally).
    Returns scipy Voronoi object with:
    - vor.vertices: Voronoi vertex coordinates
    - vor.regions: list of vertex indices per region (-1 = unbounded)
    - vor.ridge_vertices: vertex pairs defining Voronoi edges
    - vor.point_region: region index for each input site
    """
    points = np.array(sites)
    vor = Voronoi(points)
    return vor


def nearest_site(vor, sites, query_point):
    """Find which site is closest to query_point using Voronoi."""
    sites_arr = np.array(sites)
    q = np.array(query_point)
    distances = np.linalg.norm(sites_arr - q, axis=1)
    return sites[np.argmin(distances)]


# Example: compute and visualize Voronoi diagram
sites = [
    (0, 0), (4, 0), (2, 4), (2, -4), (6, 2), (-2, 2)
]
vor = compute_voronoi(sites)

print("Voronoi vertices (diagram intersections):")
print(vor.vertices)

print("\nRidge vertices (edges of Voronoi diagram):")
print(vor.ridge_vertices)    # -1 = extends to infinity

# Optional: plot
# fig, ax = plt.subplots()
# voronoi_plot_2d(vor, ax=ax)
# plt.scatter(*zip(*sites), c='red', zorder=5)
# plt.show()

# Nearest-site query
query = (1, 1)
nearest = nearest_site(vor, sites, query)
print(f"\nNearest site to {query}: {nearest}")   # (0, 0)
```

---

## 🎯 Recognize This Problem When...

- You need to find the **nearest site** for many query points.
- Keywords: "Voronoi", "nearest neighbor region", "cell coverage", "facility location".
- You're building a map showing which region each service covers.
- You need the **Delaunay triangulation** (the dual of the Voronoi diagram) for mesh generation.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Preprocessing for many nearest-neighbor queries | ✅ Build once, query O(log n) |
| Geographic service area computation | ✅ Classic GIS application |
| Game AI territory assignment | ✅ Procedural map generation |
| Single nearest-neighbor query | ❌ KD-tree or brute force is simpler |
| Non-Euclidean distance (Manhattan, etc.) | ❌ Standard Voronoi uses Euclidean only |
| 3D points | ❌ 2D algorithm doesn't generalize directly |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Closest Pair of Points](ClosestPairOfPoints.md) | Both are fundamental 2D geometry problems |
| [Convex Hull](ConvexHull.md) | Outer sites' Voronoi cells are unbounded; dual to convex hull |
| [Sweep Line](SweepLine.md) | Fortune's algorithm IS a sweep-line algorithm |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Assign Cookies](https://leetcode.com/problems/assign-cookies/) | LeetCode | 🟢 Easy (spatial assignment) |
| [Maximize Sum Of Array After K Negations](https://leetcode.com/problems/maximize-sum-of-array-after-k-negations/) | LeetCode | 🟢 Easy |
| [Voronoi Diagram](https://www.hackerrank.com/challenges/voronoi-diagrams/problem) | HackerRank | 🔴 Hard |
