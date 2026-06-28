# Bellman-Ford Alqoritmi

**Fikir:** Bəzi tilovların çəkisi mənfi ola bilər (Dijkstra burada işləmir). Bellman-Ford **hər tilovu V−1 dəfə boşaltmağa çalışır**. k-cı turdan sonra ən çox k addımda əlçatan bütün node-lar düzgün ən qısa məsafəsini alır.

## Necə işləyir
1. `dist[mənbə] = 0`, qalanları `∞`.
2. **Bütün tilovları V−1 dəfə boşalt:** hər `(u, v, w)` üçün, əgər `dist[u] + w < dist[v]` → `dist[v] = dist[u] + w`.
3. **Mənfi dövr yoxlaması — bir tur də:** hələ də boşaldıla bilən tilov varsa, mənfi dövr var və ən qısa yollar təyin olunmur.

> Niyə V−1? V node-lu qrafda ən qısa yol ən çox V−1 tilovdan keçir (artığı node-u təkrar gəzmək = dövr deməkdir).

## Nümunə
`A→B(4), A→C(5), B→C(-3), C→D(4)`, mənbə = A
- B=4, C=5 → B→C ilə C=1 → C→D ilə D=5
- Nəticə: **A=0, B=4, C=1, D=5** ✅

## Kod
```python
def bellman_ford(vertices, edges, source):
    """edges: (u, v, çəki) siyahısı."""
    dist = {v: float('inf') for v in vertices}
    dist[source] = 0
    pred = {v: None for v in vertices}
    # Bütün tilovları V−1 dəfə boşalt
    for _ in range(len(vertices) - 1):
        updated = False
        for u, v, w in edges:
            if dist[u] != float('inf') and dist[u] + w < dist[v]:
                dist[v] = dist[u] + w
                pred[v] = u
                updated = True
        if not updated:
            break                          # dəyişmə yoxdur — erkən çıx
    # Mənfi dövr yoxlaması
    neg_cycle = any(dist[u] != float('inf') and dist[u] + w < dist[v]
                    for u, v, w in edges)
    return dist, neg_cycle

vertices = ['A', 'B', 'C', 'D']
edges = [('A','B',4), ('A','C',5), ('B','C',-3), ('C','D',4)]
print(bellman_ford(vertices, edges, 'A')[0])   # {'A':0,'B':4,'C':1,'D':5}
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(V × E) |
| Yaddaş | O(V) |

Dijkstra-dan yavaşdır, amma onun bacarmadığını edir — mənfi çəkilər və dövr aşkarlama.

## Nə vaxt
- ✅ Qrafda **mənfi çəkilər** var (Dijkstra səhv cavab verər).
- ✅ **Mənfi dövr** aşkarlamaq (məs. valyuta arbitrajı).
- ✅ Qraf seyrəkdir (E kiçik), O(VE) idarə olunandır.
- ❌ Bütün çəkilər mənfi deyil — Dijkstra çox daha sürətli.
- ❌ Bütün cütlər arası yol — Floyd-Warshall.

## Əlaqəli
- [Dijkstra](Dijkstra.md) — mənfi olmayan çəkilər üçün sürətli; Bellman-Ford ehtiyat variantdır.
- [Floyd-Warshall](FloydWarshall.md) — bütün cütlər arası, həm də mənfi tilovlar.
- [BFS](BFS.md) — çəkisiz qrafda ən qısa yol.
