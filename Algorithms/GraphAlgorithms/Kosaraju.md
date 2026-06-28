# Kosaraju Alqoritmi

**Fikir:** **Güclü Əlaqəli Komponent (SCC)** — hər node-un hər digərinə çatdığı qrupdur. Açar fikir: A-dan B-yə orijinal qrafda, B-dən A-ya isə **tərs** qrafda çata bilirsənsə, onlar eyni SCC-dədir. İki keçid istifadə edir (Tarjan-dan başa düşmək asandır).

## Necə işləyir
**Keçid 1 — bitmə sırası:**
1. **Orijinal** qrafda DFS işlət.
2. Node bitəndə (bütün törəmələri gəzilib) onu **stekə** qoy.

**Keçid 2 — SCC tapma:**
3. Qrafı **tərsinə çevir** (bütün tilovların istiqamətini dəyiş).
4. Stek boşalana qədər üstdəki node-u çıxar; ziyarət olunmayıbsa, tərs qrafda ondan DFS işlət. Bu DFS-də çatılan hər node eyni SCC-dədir.

## Nümunə
`0→1, 1→2, 2→0, 2→3, 3→4, 4→5, 5→3`
- Keçid 1 bitmə steki: üstdə 5.
- Tərs qrafda 5-dən DFS → {3,4,5}; sonra 2-dən → {0,1,2}
- SCC-lər: `{3,4,5}` və `{0,1,2}` ✅

## Kod
```python
def kosaraju(graph):
    """graph: {node: [qonşular]}, tam ədəd ID-ləri."""
    V = len(graph)
    visited = [False] * V
    finish_order = []

    def dfs1(u):                          # orijinal qraf — bitmə sırası
        visited[u] = True
        for v in graph[u]:
            if not visited[v]:
                dfs1(v)
        finish_order.append(u)

    for u in range(V):
        if not visited[u]:
            dfs1(u)

    transposed = {i: [] for i in range(V)}   # tilovları tərsinə çevir
    for u in range(V):
        for v in graph[u]:
            transposed[v].append(u)

    visited = [False] * V
    sccs = []

    def dfs2(u, comp):                    # tərs qraf — bir SCC topla
        visited[u] = True
        comp.append(u)
        for v in transposed[u]:
            if not visited[v]:
                dfs2(v, comp)

    for u in reversed(finish_order):      # tərs bitmə sırası ilə
        if not visited[u]:
            comp = []
            dfs2(u, comp)
            sccs.append(comp)
    return sccs

graph = {0:[1], 1:[2], 2:[0,3], 3:[4], 4:[5], 5:[3]}
print(kosaraju(graph))   # məs. [[0,1,2],[3,4,5]]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(V + E) (iki DFS) |
| Yaddaş | O(V + E) (tərs qraf) |

## Nə vaxt
- ✅ Yönlü qrafda qarşılıqlı çatılan node qruplarını sadə, aydın kodla tapmaq.
- ✅ Bütün SCC istifadə halları (2-SAT, kondensasiya).
- ❌ Yaddaş məhduddur — tərs qraf əlavə O(V+E) yer tutur.
- ❌ Bir keçiddə maksimum səmərə lazımdır — Tarjan bir DFS işlədir.

## Əlaqəli
- [Tarjan](Tarjan.md) — həm də SCC; tək DFS (daha səmərəli, az intuitiv).
- [DFS](DFS.md) — Kosaraju-nun hər iki keçidi adi DFS-dir.
- [Topological Sort](TopologicalSort.md) — SCC node kimi götürülən kondensasiya qrafı DAG-dır.
