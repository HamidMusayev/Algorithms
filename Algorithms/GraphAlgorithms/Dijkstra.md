### **Dijkstra's Algorithm 🌐**

Finds the **shortest path** from a source node to all other nodes in a **weighted graph with non-negative edges**. Greedy approach using a min-heap.

---

#### **Summary**
- **Time Complexity:** O((V + E) log V) with a binary heap
- **Space Complexity:** O(V)
- **Constraint:** ❗ Does **not** work with negative edge weights (use Bellman-Ford).

---

#### **How It Works**
1. Initialize distances: 0 for source, ∞ for others.
2. Use a min-heap of `(distance, node)`.
3. Pop the smallest; for each neighbor, **relax** its distance if shorter.
4. Repeat until heap is empty.

---

#### **Python Implementation**
```python
import heapq

def dijkstra(graph, start):
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    pq = [(0, start)]

    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]:
            continue
        for neighbor, weight in graph[node]:
            nd = d + weight
            if nd < dist[neighbor]:
                dist[neighbor] = nd
                heapq.heappush(pq, (nd, neighbor))
    return dist

# Example: graph = { node: [(neighbor, weight), ...] }
graph = {
    'A': [('B', 1), ('C', 4)],
    'B': [('C', 2), ('D', 5)],
    'C': [('D', 1)],
    'D': []
}
print(dijkstra(graph, 'A'))   # {'A':0,'B':1,'C':3,'D':4}
```

---

#### **When to Use**
✅ Shortest paths in road networks, routing
✅ All non-negative weighted graphs
🚫 Negative edges → use Bellman-Ford
🚫 All-pairs → use Floyd-Warshall
