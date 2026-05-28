### **Voronoi Diagram 📏**

Given a set of "seed" points (sites), the Voronoi diagram partitions the plane into **regions** where each region contains exactly the set of points closer to one seed than to any other.

The **dual** of a Voronoi diagram is the **Delaunay triangulation**.

---

#### **Summary**
- **Optimal construction:** O(n log n) via **Fortune's sweep-line algorithm**
- **Space Complexity:** O(n)
- **Output:** edges, vertices, and cells of the diagram

Fortune's algorithm is non-trivial to implement from scratch — in practice, use **scipy**.

---

#### **Python Implementation (via SciPy)**
```python
import numpy as np
from scipy.spatial import Voronoi

points = np.array([
    [0, 0], [2, 0], [1, 1],
    [0, 2], [2, 2], [1, 3],
])

vor = Voronoi(points)
print("Vertices:")
print(vor.vertices)              # Voronoi vertex coordinates
print("Regions:")
print(vor.regions)               # vertex indices per region (-1 = open)
print("Ridges:")
print(vor.ridge_vertices)        # vertex pairs defining edges
```

> SciPy uses **Qhull** under the hood (O(n log n)).

---

#### **When to Use**
✅ Nearest-neighbor queries, mesh generation
✅ GIS / spatial analysis (cell coverage, facility location)
✅ Procedural terrain / textures in games
🚫 Implementing Fortune's from scratch is heavy — prefer libraries
