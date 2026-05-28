### **Prim's Algorithm 🌐**

Builds a **Minimum Spanning Tree (MST)** of a connected, undirected, weighted graph by **growing** the tree one cheapest edge at a time.

---

#### **Summary**
- **Time Complexity:** O((V + E) log V) with a min-heap
- **Space Complexity:** O(V + E)
- **Approach:** Greedy

---

#### **How It Works**
1. Start with any node.
2. Repeatedly pick the **cheapest edge** that connects a visited node to an unvisited one.
3. Add it to the MST and mark the new node visited.
4. Stop when all nodes are included.

---

#### **Python Implementation**
```python
import heapq

def prim(graph, start):
    # graph[node] = [(neighbor, weight), ...]
    visited = {start}
    edges = [(w, start, v) for v, w in graph[start]]
    heapq.heapify(edges)
    mst, total = [], 0

    while edges and len(visited) < len(graph):
        w, u, v = heapq.heappop(edges)
        if v in visited:
            continue
        visited.add(v)
        mst.append((u, v, w))
        total += w
        for nxt, nw in graph[v]:
            if nxt not in visited:
                heapq.heappush(edges, (nw, v, nxt))
    return mst, total

# Example
graph = {
    'A': [('B', 1), ('C', 3)],
    'B': [('A', 1), ('C', 1), ('D', 4)],
    'C': [('A', 3), ('B', 1), ('D', 1)],
    'D': [('B', 4), ('C', 1)]
}
print(prim(graph, 'A'))   # ([('A','B',1),('B','C',1),('C','D',1)], 3)
```

---

#### **When to Use**
✅ Dense graphs (more edges than nodes)
✅ When starting from a specific node makes sense
🚫 For sparse graphs Kruskal's may be simpler
