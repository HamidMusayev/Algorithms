# Dijkstra Alqoritmi

**Fikir:** Mənbədən çıxan su həmişə əvvəlcə ən ucuz borudan axır. Dijkstra hər node-a "ən yaxşı məlum məsafə"ni saxlayır və həmişə hazırda ən ucuz əlçatan node-u emal edir. Node emal olunan kimi onun ən qısa məsafəsi həmişəlik təsdiqlənir. **Çəkilər mənfi olmamalıdır.**

## Necə işləyir
1. Məsafələri başlat: `dist[mənbə] = 0`, qalanları `∞`.
2. `(0, mənbə)`-ni **min-topaya** (prioritet növbə) qoy.
3. Topa boşalana qədər:
   - Ən kiçik məsafəli `u` node-u çıxar.
   - `d > dist[u]`-dursa, atla (artıq daha qısa yol tapılıb).
   - `u`-nun hər `(v, w)` qonşusu üçün, əgər `dist[u] + w < dist[v]` → **tilovu boşalt**: `dist[v]`-ni yenilə və topaya qoy.

## Nümunə
A→B(1), A→C(4), B→C(2). Mənbə: **A**
- A çıxır: B=1, C=4
- B çıxır: C = 1+2 = 3 < 4 → yenilə
- Nəticə: **A=0, B=1, C=3** ✅

## Kod
```python
import heapq

def dijkstra(graph, start):
    """graph = { node: [(qonşu, çəki), ...] }"""
    dist = {node: float('inf') for node in graph}
    dist[start] = 0
    pq = [(0, start)]                       # min-topa: (məsafə, node)
    while pq:
        d, node = heapq.heappop(pq)
        if d > dist[node]:                  # köhnəlmiş giriş — atla
            continue
        for neighbor, weight in graph[node]:
            new_dist = d + weight           # tilovu boşalt
            if new_dist < dist[neighbor]:
                dist[neighbor] = new_dist
                heapq.heappush(pq, (new_dist, neighbor))
    return dist

graph = {'A': [('B',1),('C',4)], 'B': [('C',2),('D',5)], 'C': [('D',1)], 'D': []}
print(dijkstra(graph, 'A'))   # {'A':0,'B':1,'C':3,'D':4}
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O((V + E) log V) (binar topa) |
| Yaddaş | O(V) |

## Nə vaxt
- ✅ Tək mənbədən ən qısa yol, **mənfi olmayan** çəkilər (yol şəbəkəsi, GPS).
- ❌ Bütün çəkilər 1-dir — BFS daha sadə və eyni mürəkkəblik.
- ❌ Mənfi çəkilər — [Bellman-Ford](BellmanFord.md).
- ❌ Bütün cütlər arası ən qısa yol — [Floyd-Warshall](FloydWarshall.md).

## Əlaqəli
- [BFS](BFS.md) — bütün çəkilər = 1 olan Dijkstra.
- [Bellman-Ford](BellmanFord.md) — mənfi tilovları idarə edir; daha yavaş O(VE).
- [A*](AStar.md) — Dijkstra + heuristika; hədəf məlum olanda daha sürətli.
- [Prim](Prim.md) — eyni min-topa quruluşu, amma MST qurur.
