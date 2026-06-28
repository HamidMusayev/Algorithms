# Breadth-First Search (BFS)

**Fikir:** Suya atılan daşın dairəvi dalğaları kimi — başlanğıc node-dan əvvəlcə bütün 1 məsafəli qonşular, sonra 2 məsafəlilər gəzilir. "Əvvəlcə dostlar, sonra dostların dostları."

## Necə işləyir
1. Başlanğıc node-u növbəyə (queue) əlavə et və ziyarət olunmuş kimi işarələ.
2. Növbə boşalana qədər:
   - Öndəki node-u çıxar, emal et.
   - Hər ziyarət olunmamış qonşunu işarələ və növbəyə əlavə et.
3. Növbə boşaldıqda bütün əlçatan node-lar səviyyə-səviyyə ziyarət olunub.

> Növbə (FIFO) node-ların aşkarlanma sırası ilə emalını təmin edir — yəni səviyyə-səviyyə.

## Nümunə
Graf: A–B, A–C, B–D, B–E, C–E. Başlanğıc: **A**

BFS sırası: A → B → C → D → E
A-dan məsafələr: A=0, B=1, C=1, D=2, E=2

## Kod
```python
from collections import deque

def bfs(graph, start):
    visited = {start}
    queue = deque([start])       # deque → O(1) popleft
    order = []
    while queue:
        node = queue.popleft()   # öndən çıxar (FIFO)
        order.append(node)
        for neighbor in graph[node]:
            if neighbor not in visited:
                visited.add(neighbor)   # əlavə etməzdən əvvəl işarələ
                queue.append(neighbor)
    return order

def bfs_distances(graph, start):
    """Hər node-a qədər addım sayı."""
    dist = {start: 0}
    queue = deque([start])
    while queue:
        node = queue.popleft()
        for neighbor in graph[node]:
            if neighbor not in dist:
                dist[neighbor] = dist[node] + 1
                queue.append(neighbor)
    return dist

graph = {'A': ['B','C'], 'B': ['A','D','E'], 'C': ['A','E'], 'D': ['B'], 'E': ['B','C']}
print(bfs(graph, 'A'))              # ['A', 'B', 'C', 'D', 'E']
print(bfs_distances(graph, 'A'))   # {'A':0,'B':1,'C':1,'D':2,'E':2}
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(V + E) |
| Yaddaş | O(V) |

V = təpə (vertex), E = tilov (edge) sayı. Hər node və tilov ən çox bir dəfə ziyarət olunur.

## Nə vaxt
- ✅ **Çəkisiz** qrafda və ya gridda ən qısa yol / minimum addım sayı.
- ✅ Ağacın səviyyə-səviyyə gəzilməsi; "ən yaxın" obyekti tapmaq (ən yaxın çıxış, ada).
- ✅ İki-rənglilik (bipartite) yoxlaması.
- ❌ **Çəkili** qrafda ən qısa yol — Dijkstra istifadə et.
- ❌ Yalnız yolun mövcudluğunu yoxlamaq lazımdırsa — DFS daha az yaddaş işlədir.

## Əlaqəli
- [DFS](DFS.md) — digər əsas gəzinti; dərinə gedir.
- [Dijkstra](Dijkstra.md) — BFS-i prioritet növbə ilə çəkili qrafa genişləndirir.
- [A*](AStar.md) — Dijkstra + heuristika ilə məqsədə yönəlir.
- [Topological Sort](TopologicalSort.md) — Kahn alqoritmi giriş-dərəcəsi üzərində BFS işlədir.
