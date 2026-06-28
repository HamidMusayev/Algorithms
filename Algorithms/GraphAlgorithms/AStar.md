# A* (A-star) Axtarışı

**Fikir:** Dijkstra hər istiqamətə bərabər yayılır. A* daha ağıllıdır — **heuristika** (qalan məsafənin təxmini) ilə axtarışı məqsədə yönəldir. "Dijkstra + kompas."

## Necə işləyir
Hər `n` node üçün A* saxlayır:
- `g(n)` = start-dan n-ə real xərc
- `h(n)` = n-dən məqsədə heuristik təxmin
- `f(n) = g(n) + h(n)` = n-dən keçən yolun təxmini ümumi xərci

1. Start-ı **açıq çoxluğa** (f-ə görə sıralanmış prioritet növbə) əlavə et.
2. Açıq çoxluq boşalana qədər:
   - Ən kiçik `f(n)`-li node-u çıxar.
   - Məqsəddirsə → yolu qur və qaytar.
   - Hər qonşu üçün `g = g(cari) + tilov_çəkisi` hesabla; daha yaxşıdırsa yenilə və əlavə et.

**Tez-tez işlənən heuristikalar:** grid üçün Manhattan `|x₁−x₂|+|y₁−y₂|`; fəzada Evklid `√((x₁−x₂)²+(y₁−y₂)²)`.

## Nümunə
A→B(1)→C(2)→D(1), məqsəd D. h = D-yə Manhattan: h(A)=3, h(B)=2, h(C)=1, h(D)=0.
A → B → C → D, xərc = 4 ✅

## Kod
```python
import heapq

def a_star(graph, start, goal, heuristic):
    """graph: {node: [(qonşu, xərc), ...]}; heuristic(node) → məqsədə təxmin."""
    open_set = [(heuristic(start), 0, start, [start])]   # (f, g, node, yol)
    best_g = {}
    while open_set:
        f, g, node, path = heapq.heappop(open_set)
        if node == goal:
            return g, path                    # optimal yol tapıldı
        if node in best_g and best_g[node] <= g:
            continue                          # bu node-a daha yaxşı yol var
        best_g[node] = g
        for neighbor, cost in graph[node]:
            new_g = g + cost
            new_f = new_g + heuristic(neighbor)
            heapq.heappush(open_set, (new_f, new_g, neighbor, path + [neighbor]))
    return float('inf'), []

graph = {'A': [('B',1),('C',4)], 'B': [('C',2),('D',5)], 'C': [('D',1)], 'D': []}
h = {'A': 3, 'B': 2, 'C': 1, 'D': 0}
print(a_star(graph, 'A', 'D', lambda n: h[n]))   # (4, ['A','B','C','D'])
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | ən yaxşı O(E); ən pis O(b^d) |
| Yaddaş | O(V) |

Heuristika **mümkün (admissible)** olsa (heç vaxt çox təxmin etməsə) — A* optimaldır. `h=0` olanda A* sadəcə Dijkstra-ya çevrilir.

## Nə vaxt
- ✅ Konkret bir **məqsədə** ən qısa yol (bütün node-lara yox).
- ✅ Məkan xəritəsi/grid var və məqsədə məsafə təxmin edilə bilir (oyun, robot naviqasiyası).
- ❌ Bütün node-lara yol lazımdır — Dijkstra.
- ❌ Yaxşı heuristika yoxdur (h=0) — sadəcə Dijkstra işlət.

## Əlaqəli
- [Dijkstra](Dijkstra.md) — `h=0` olan A*; hər istiqamətə bərabər yayılır.
- [BFS](BFS.md) — çəkisiz qrafda h=0 olan A*.
