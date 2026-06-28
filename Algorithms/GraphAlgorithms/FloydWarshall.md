# Floyd-Warshall Alqoritmi

**Fikir:** Hər şəhər cütü arasında birbaşa uçuş qiymətləri var. İstənilən iki şəhər arasında (bəlkə aralıq dayanacaqlarla) ən ucuz yolu istəyirik. Floyd-Warshall hər node-u **aralıq dayanacaq** kimi yoxlayır: "k-dan keçmək cari i→j yolundan ucuzdurmu?"

## Necə işləyir
1. V×V məsafə matrisi qur: `dist[i][i]=0`, tilov varsa `dist[i][j]=çəki`, yoxsa `∞`.
2. Hər aralıq node **k** (0..V−1) üçün, hər **(i, j)** cütü üçün:
   - `dist[i][k] + dist[k][j] < dist[i][j]` → `dist[i][j]`-ni yenilə.
3. Bütün k-lardan sonra `dist[i][j]` i→j ən qısa yoludur.
4. **Mənfi dövr:** hər hansı `dist[i][i] < 0`-dırsa, mənfi dövr var.

## Nümunə
4 node-lu qrafda hər k aralıq dayanacaq kimi yoxlanır. Məs. 0→2 birbaşa yox idi, amma k=1 ilə 0→1→2 = 3+2 = 5 tapılır.

## Kod
```python
def floyd_warshall(graph):
    """graph: V×V matris, graph[i][j] = i→j çəkisi və ya inf."""
    V = len(graph)
    dist = [row[:] for row in graph]        # orijinalı dəyişmə
    for i in range(V):
        dist[i][i] = 0
    for k in range(V):                       # hər aralıq node
        for i in range(V):
            for j in range(V):
                if dist[i][k] + dist[k][j] < dist[i][j]:
                    dist[i][j] = dist[i][k] + dist[k][j]
    for i in range(V):                       # mənfi dövr yoxlaması
        if dist[i][i] < 0:
            raise ValueError(f"Mənfi dövr: node {i}")
    return dist

INF = float('inf')
graph = [[0,3,INF,7], [8,0,2,INF], [5,INF,0,1], [2,INF,INF,0]]
for row in floyd_warshall(graph):
    print(row)
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(V³) |
| Yaddaş | O(V²) |

O(V³) olduğu üçün yalnız kiçik/orta qraflarda (V ≤ ~500) praktikdir.

## Nə vaxt
- ✅ **Bütün cütlər** arası ən qısa yol (tək mənbə yox).
- ✅ Qraf kiçikdir; mənfi çəkilər var (amma mənfi dövr yox); tranzitiv qapanma.
- ❌ Böyük qraf (V > 1000) — O(V³) yavaşdır.
- ❌ Yalnız tək mənbədən yol lazımdır — Dijkstra/Bellman-Ford daha sürətli.

## Əlaqəli
- [Dijkstra](Dijkstra.md) — tək mənbə; mənfi olmayan çəkilərdə daha sürətli.
- [Bellman-Ford](BellmanFord.md) — tək mənbə; mənfi tilovlar; O(VE).
- [BFS](BFS.md) — çəkisiz qrafda hər node-dan işlədilərək bütün cütlər.
