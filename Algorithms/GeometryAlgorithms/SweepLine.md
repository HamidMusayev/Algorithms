### **Sweep Line Algorithms 📏**

A general **paradigm** rather than one algorithm: imagine an imaginary vertical line sweeping across the plane left to right, processing **events** (segment endpoints, etc.) and maintaining an active set in a balanced BST.

Famous examples:
- **Bentley-Ottmann** – all segment intersections in O((n + k) log n)
- **Fortune's algorithm** – Voronoi diagrams in O(n log n)
- **Closest pair** (sweep variant) – O(n log n)
- **Skyline problem**, **rectangle union area**

---

#### **General Pattern**
1. Collect events (endpoints, breakpoints).
2. Sort by x-coordinate.
3. Sweep, maintaining a "status" structure of active objects.
4. At each event: insert / delete / report intersections.

---

#### **Python Example — Skyline Problem**

The classic LeetCode skyline problem uses a sweep with a max-heap:

```python
import heapq

def get_skyline(buildings):
    # buildings = [(left, right, height), ...]
    events = []
    for L, R, H in buildings:
        events.append((L, -H, R))
        events.append((R, 0, 0))
    events.sort()

    result = []
    heap = [(0, float('inf'))]   # (-height, end)
    for x, negH, R in events:
        if negH:
            heapq.heappush(heap, (negH, R))
        # remove buildings that have ended
        while heap[0][1] <= x:
            heapq.heappop(heap)
        h = -heap[0][0]
        if not result or result[-1][1] != h:
            result.append([x, h])
    return result

# Example
buildings = [(2,9,10), (3,7,15), (5,12,12), (15,20,10), (19,24,8)]
print(get_skyline(buildings))
# [[2,10], [3,15], [7,12], [12,0], [15,10], [20,8], [24,0]]
```

---

#### **When to Use**
✅ Geometric problems with events along an axis
✅ Range / interval problems
✅ Anything reducible to "process in order, maintain a structure"
