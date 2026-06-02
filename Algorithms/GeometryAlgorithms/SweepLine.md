# Sweep Line Algorithm 🧹

---

## 🧠 Intuition

Imagine a vertical line scanning left to right across a plane. As it moves, it triggers **events** (endpoints of segments, intersections, etc.) and you process each event to answer questions about the geometric objects it crosses.

The sweep line is a **general paradigm**, not one specific algorithm. Classic uses:
- **Bentley-Ottmann**: find all segment-segment intersections in O((n+k) log n)
- **Fortune's algorithm**: compute Voronoi diagrams in O(n log n)
- **Skyline problem**: find the silhouette of buildings
- **Rectangle union area**: compute total area of overlapping rectangles

**Mental model:** "Turn a 2D geometry problem into a 1D problem evolving over time. Process discrete events in sorted order."

---

## 📊 Complexity

| Problem | Time | Notes |
|---------|------|-------|
| Segment intersections (Bentley-Ottmann) | O((n+k) log n) | k = # intersections |
| Skyline problem | O(n log n) | n = buildings |
| Rectangle union area | O(n log n) | n = rectangles |
| Fortune's Voronoi | O(n log n) | n = sites |

---

## ⚙️ How It Works

**General Pattern:**
1. **Create events** — all discrete moments when something changes (endpoints, intersection points, breakpoints).
2. **Sort events** by x-coordinate (and break ties by y or event type).
3. **Maintain a status structure** — a balanced BST (or sorted list) of active objects intersecting the sweep line, ordered by their y-coordinate at the sweep line.
4. **Process each event:**
   - **Start event**: insert the object into the status structure; check for new intersections with neighbors.
   - **End event**: remove the object; check if its former neighbors now intersect.
   - **Intersection event**: swap the two segments; recheck their new neighbors.

---

## 🔢 Step-by-Step Trace

**Skyline Problem** with buildings `[(2,9,10), (3,7,15), (5,12,12)]` (left, right, height):

Events (sorted by x):
```
x=2: start building A (h=10) → active={A}   → skyline changes to 10
x=3: start building B (h=15) → active={A,B} → skyline changes to 15
x=5: start building C (h=12) → active={A,B,C} → no change (B still tallest)
x=7: end building B (h=15)   → active={A,C} → skyline drops to 12
x=9: end building A (h=10)   → active={C}   → no change (C still 12)
x=12: end building C (h=12)  → active={}    → skyline drops to 0
```

Skyline key points: `(2,10),(3,15),(7,12),(9,12),(12,0)` → simplified to `[(2,10),(3,15),(7,12),(12,0)]`

---

## 🐍 Python Implementation

```python
import heapq

def skyline(buildings):
    """
    Skyline Problem using sweep line + max-heap.
    buildings: [(left, right, height), ...]
    Returns list of [x, height] key points.
    """
    # Create events: (x, -height, right) for starts, (x, 0, 0) for ends
    events = []
    for L, R, H in buildings:
        events.append((L, -H, R))    # Building starts: negative height for min-heap trick
        events.append((R, 0, 0))     # Building ends
    events.sort()                     # Sort by x, then by height (tallest first at ties)

    result = []
    # Max-heap: store (-height, end) — negate height to simulate max-heap with heapq
    heap = [(0, float('inf'))]        # Sentinel: ground level, never ends

    for x, neg_h, R in events:
        if neg_h:                     # Building start event
            heapq.heappush(heap, (neg_h, R))

        # Remove buildings that have already ended (lazy deletion)
        while heap[0][1] <= x:
            heapq.heappop(heap)

        # Current max height at this x position
        curr_height = -heap[0][0]

        # Only record a key point if height changed
        if not result or result[-1][1] != curr_height:
            result.append([x, curr_height])

    return result


def segment_intersection_exists(segments):
    """
    Check if ANY two segments in the list intersect — simple O(n² log n) version.
    For full Bentley-Ottmann, use a specialized library.
    """
    from itertools import combinations

    def orient(p, q, r):
        val = (q[0]-p[0])*(r[1]-p[1]) - (q[1]-p[1])*(r[0]-p[0])
        return 1 if val > 0 else (-1 if val < 0 else 0)

    def on_seg(p, q, r):
        return min(p[0],r[0]) <= q[0] <= max(p[0],r[0]) and \
               min(p[1],r[1]) <= q[1] <= max(p[1],r[1])

    def intersect(s1, s2):
        p1,p2 = s1; p3,p4 = s2
        o = [orient(p1,p2,p3), orient(p1,p2,p4), orient(p3,p4,p1), orient(p3,p4,p2)]
        if o[0]!=o[1] and o[2]!=o[3]: return True
        if o[0]==0 and on_seg(p1,p3,p2): return True
        if o[1]==0 and on_seg(p1,p4,p2): return True
        if o[2]==0 and on_seg(p3,p1,p4): return True
        if o[3]==0 and on_seg(p3,p2,p4): return True
        return False

    return any(intersect(s1,s2) for s1,s2 in combinations(segments, 2))


# Examples
buildings = [(2, 9, 10), (3, 7, 15), (5, 12, 12), (15, 20, 10), (19, 24, 8)]
print("Skyline:", skyline(buildings))
# [[2, 10], [3, 15], [7, 12], [12, 0], [15, 10], [20, 8], [24, 0]]

segs = [((1,1),(5,5)), ((1,5),(5,1)), ((6,1),(8,3))]
print("Any intersection:", segment_intersection_exists(segs))  # True
```

---

## 🎯 Recognize This Problem When...

- The problem involves **intervals, segments, or events** ordered along one axis.
- You need to find **what overlaps** at each x-position.
- Keywords: "skyline", "sweep", "events at x-position", "active objects".
- Counting rectangles, computing area of union, finding intersections.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Interval/segment problems with events | ✅ Natural fit |
| Skyline, rectangle area union | ✅ Classic sweep line problems |
| Finding all intersections (n segments) | ✅ Bentley-Ottmann |
| Simple single-pair intersection | ❌ Use O(1) orient test directly |
| 3D geometric problems | ❌ 2D sweep doesn't generalize easily |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Line Intersection](LineIntersection.md) | The O(1) intersection check used inside sweep line |
| [Voronoi Diagram](VoronoiDiagram.md) | Fortune's algorithm IS a sweep line algorithm |
| [Convex Hull](ConvexHull.md) | Andrew's monotone chain is also a "sweep" (left to right) |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [The Skyline Problem](https://leetcode.com/problems/the-skyline-problem/) | LeetCode | 🔴 Hard |
| [Rectangle Area II](https://leetcode.com/problems/rectangle-area-ii/) | LeetCode | 🔴 Hard |
| [Count of Smaller Numbers After Self](https://leetcode.com/problems/count-of-smaller-numbers-after-self/) | LeetCode | 🔴 Hard |
