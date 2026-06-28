# Tarjan Alqoritmi

**Fikir:** **Güclü Əlaqəli Komponent (SCC)** — hər node-dan hər digərinə çatmaq mümkün olan node qrupudur (yönlü qrafda qarşılıqlı çatma "adası"). Tarjan bir DFS-də bütün SCC-ləri tapır.

Açar fikir: DFS zamanı node `u` üçün `disc[u] == low[u]`-dursa (aşkarlanma vaxtı = alt-ağacdan çatıla bilən ən kiçik vaxt), `u` bir SCC-nin **kökü**dür.

## Necə işləyir
1. Hər ziyarət olunmamış node-dan DFS başlat.
2. Hər node-a **aşkarlanma vaxtı** `disc[u]` və **low dəyəri** `low[u]` ver (əvvəlcə bərabər).
3. Node-u **stekə** qoy və `on_stack` işarələ.
4. Hər `v` qonşusu üçün:
   - Ziyarət olunmayıbsa: rekursiya; sonra `low[u] = min(low[u], low[v])`.
   - Stekdədirsə: `low[u] = min(low[u], disc[v])` (geri-tilov → eyni SCC).
5. `low[u] == disc[u]` → **SCC kökü**: stekdən `u` çıxana qədər node-ları çıxar. O qrup bir SCC-dir.

## Nümunə
`0→1, 1→2, 2→0, 2→3, 3→4, 4→5, 5→3`
İki SCC: `{0,1,2}` və `{3,4,5}` ✅

## Kod
```python
def tarjan_scc(graph):
    """graph: {node: [qonşular]}, tam ədəd node ID-ləri."""
    V = len(graph)
    disc = [-1] * V           # aşkarlanma vaxtı (-1 = ziyarət olunmayıb)
    low = [0] * V
    on_stack = [False] * V
    stack, sccs, timer = [], [], [0]

    def dfs(u):
        disc[u] = low[u] = timer[0]
        timer[0] += 1
        stack.append(u)
        on_stack[u] = True
        for v in graph[u]:
            if disc[v] == -1:
                dfs(v)
                low[u] = min(low[u], low[v])      # ağac-tilovu
            elif on_stack[v]:
                low[u] = min(low[u], disc[v])     # geri-tilov
        if low[u] == disc[u]:                     # SCC kökü
            scc = []
            while True:
                w = stack.pop()
                on_stack[w] = False
                scc.append(w)
                if w == u:
                    break
            sccs.append(scc)

    for u in range(V):
        if disc[u] == -1:
            dfs(u)
    return sccs

graph = {0:[1], 1:[2], 2:[0,3], 3:[4], 4:[5], 5:[3]}
print(tarjan_scc(graph))   # məs. [[5,4,3],[2,1,0]]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(V + E) (bir DFS) |
| Yaddaş | O(V) |

## Nə vaxt
- ✅ Yönlü qrafda qarşılıqlı çatılan node qruplarını tapmaq.
- ✅ 2-SAT, kondensasiya qrafı (dövrləri sıxmaq), tsiklik asılılıqlar.
- ❌ Yönsüz qraf (sadəcə əlaqəli komponentlər) — adi BFS/DFS.
- ❌ Daha sadə kod istəyirsən — Kosaraju daha asandır.

## Əlaqəli
- [Kosaraju](Kosaraju.md) — həm də SCC tapır; 2 DFS keçidi (başa düşmək daha asan).
- [DFS](DFS.md) — Tarjan əlavə qeydlərlə ixtisaslaşmış DFS-dir.
- [Topological Sort](TopologicalSort.md) — SCC-lər node kimi götürülən kondensasiya qrafı həmişə DAG-dır.
