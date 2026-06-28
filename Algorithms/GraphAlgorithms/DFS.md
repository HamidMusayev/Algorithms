# Depth-First Search (DFS)

**Fikir:** Labirintdə bir dəhlizi sonacan gedirsən, dalana dirənəndə geri qayıdıb növbəti dəhlizi yoxlayırsan. "Əvvəlcə dərinə get, ilişəndə geri qayıt."

## Necə işləyir
**Rekursiv:**
1. Cari node-u ziyarət olunmuş işarələ və emal et.
2. Hər ziyarət olunmamış qonşu üçün rekursiv DFS çağır.
3. Bütün qonşular bitəndə qayıt (geri qayıt — backtrack).

**İterativ (açıq stek):** start-ı stekə qoy; stek boşalana qədər üstdəkini çıxar, ziyarət olunmayıbsa işarələ və qonşularını stekə yığ.

> BFS-dən əsas fərq: DFS **stek (LIFO)**, BFS **növbə (FIFO)** istifadə edir.

## Nümunə
Graf: A→B, A→C, B→D, B→E, C→E. Başlanğıc: **A**
DFS sırası: A → B → D → E → C

## Kod
```python
def dfs_recursive(graph, start, visited=None):
    if visited is None:
        visited = set()
    visited.add(start)
    print(start, end=' ')
    for neighbor in graph[start]:
        if neighbor not in visited:
            dfs_recursive(graph, neighbor, visited)   # geri qayıtmadan dərinə get
    return visited

def has_cycle(graph):
    """Yönlü qrafda dövr var? 0=ziyarət olunmayıb, 1=stekdə, 2=bitib."""
    state = {node: 0 for node in graph}
    def dfs(u):
        state[u] = 1
        for v in graph[u]:
            if state[v] == 1:               # geri-tilov → dövr
                return True
            if state[v] == 0 and dfs(v):
                return True
        state[u] = 2
        return False
    return any(dfs(n) for n in graph if state[n] == 0)

graph = {'A': ['B','C'], 'B': ['D','E'], 'C': ['E'], 'D': [], 'E': []}
dfs_recursive(graph, 'A')          # A B D E C
print(has_cycle({'A':['B'],'B':['C'],'C':['A']}))   # True
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(V + E) |
| Yaddaş | O(V) (rekursiya/stek) |

## Nə vaxt
- ✅ İki node arasında bütün yolları tapmaq; yolun mövcudluğu (əlaqəlilik).
- ✅ Dövr aşkarlama; topoloji sıralama (post-order); əlaqəli komponentlər.
- ✅ Rekursiv strukturlar (ağaclar, adalar).
- ❌ Çəkisiz qrafda ən qısa yol — BFS (DFS ən qısanı zəmanət vermir).
- ❌ Çox dərin qraf (stek daşması riski) — iterativ DFS istifadə et.

## Əlaqəli
- [BFS](BFS.md) — eninə gəzir; çəkisizdə ən qısa yol verir.
- [Topological Sort](TopologicalSort.md) — DAG-da post-order DFS topoloji sıra verir.
- [Tarjan](Tarjan.md) · [Kosaraju](Kosaraju.md) — güclü əlaqəli komponentlər üçün DFS istifadə edir.
