# Prim Alqoritmi

**Fikir:** Bir node-dan başlayırıq və həmişə mövcud şəbəkəyə **ən ucuz qoşulmamış node-u** birləşdiririk. Dövr yaradan tilov heç vaxt əlavə edilmir. MST-ni bitki kimi "böyüdürük". Nəticə: **Minimal Örtən Ağac (MST)**.

## Necə işləyir
1. İxtiyari bir node-dan başla (ziyarət olunmuş işarələ).
2. Bu node-dan çıxan bütün tilovları **min-topaya** (çəkiyə görə) qoy.
3. Topa boşalana və MST tamamlanana qədər:
   - Ən ucuz `(çəki, u, v)` tilovu çıxar.
   - `v` artıq ziyarət olunubsa, atla (dövr olardı).
   - Yoxsa: tilovu MST-yə əlavə et, `v`-ni işarələ, `v`-dən çıxan tilovları topaya qoy.
4. Bütün V node MST-də olanda dayan.

## Nümunə
A–B(1), A–C(3), B–C(1), B–D(4). Start: A
- A-B(1) → A-C(3) atlanır (B-C(1) ucuz) → B-C(1) → B-D(4)
- MST = {A-B, B-C, B-D}, ümumi çəki = **6**

## Kod
```python
import heapq

def prim(graph, start):
    """graph: {node: [(qonşu, çəki), ...]}"""
    visited = {start}
    heap = [(w, start, v) for v, w in graph[start]]   # (çəki, kimdən, kimə)
    heapq.heapify(heap)
    mst, total = [], 0
    while heap and len(visited) < len(graph):
        w, u, v = heapq.heappop(heap)     # ən ucuz tilov
        if v in visited:                  # hədəf artıq MST-də
            continue
        visited.add(v)
        mst.append((u, v, w))
        total += w
        for neighbor, weight in graph[v]:
            if neighbor not in visited:
                heapq.heappush(heap, (weight, v, neighbor))
    return mst, total

graph = {'A': [('B',1),('C',3)], 'B': [('A',1),('C',1),('D',4)],
         'C': [('A',3),('B',1)], 'D': [('B',4)]}
print(prim(graph, 'A'))   # ([('A','B',1),('B','C',1),('B','D',4)], 6)
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O((V + E) log V) (binar topa) |
| Yaddaş | O(V + E) |

## Nə vaxt
- ✅ Bütün node-ları minimum çəki ilə qoşmaq (MST); qraf **sıxdır** (E ≈ V²).
- ✅ Konkret node-dan başlayıb böyütmək lazımdır.
- ❌ Seyrək qraf — Kruskal daha sadə və sürətli.
- ❌ Yönlü və ya əlaqəsiz qraf.

## Əlaqəli
- [Kruskal](Kruskal.md) — həm də MST; ağac böyütmək yox, tilovları qlobal sıralayır.
- [Dijkstra](Dijkstra.md) — eyni min-topa quruluşu, amma ən qısa yol tapır, MST yox.
