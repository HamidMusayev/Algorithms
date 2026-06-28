# Hamiltonian Cycle (Hamilton Dövrü)

**Fikir:** Hamilton dövrü **hər təpəni dəqiq bir dəfə** ziyarət edib başlanğıca qayıdır (kommivoyajyor hər şəhəri bir dəfə gəzib evə qayıdır kimi). Eyler dövründən fərqli (o hər **tilovu** bir dəfə keçir) — bu hər **təpəni** bir dəfə tələb edir. NP-tamdır, ona görə backtracking işlədirik.

## Necə işləyir
1. Təpə 0-dan başla (bütün dövrlər 0-ı əhatə edir).
2. Yolun hər mövqeyi (1..N−1) üçün:
   - Hər `v` təpəni sına ki: əvvəlki təpədən tilovu olsun və hələ ziyarət olunmasın.
   - `v`-ni yola **yerləşdir**.
   - Növbəti mövqe üçün **rekursiya** et.
   - Bütün N təpə yerləşib VƏ son təpədən 0-a tilov varsa → **dövr tapıldı!**
   - Yoxsa → **geri al**: `v`-ni çıxar, növbətini sına.

## Nümunə
5 təpəli qrafda: yol `[0, 1, 2, 4, 3]` qurulur, 3→0 tilovu var →
Dövr: **0 → 1 → 2 → 4 → 3 → 0** ✅

## Kod
```python
def hamiltonian_cycle(graph):
    """graph: N×N qonşuluq matrisi (graph[u][v]=1 → u→v tilovu)."""
    n = len(graph)
    path = [-1] * n
    path[0] = 0                              # təpə 0-dan başla
    if backtrack(graph, path, 1):
        return path + [path[0]]              # dövrü göstərmək üçün başı əlavə et
    return None

def is_safe(graph, path, pos, v):
    if graph[path[pos - 1]][v] == 0:         # əvvəlki təpədən tilov yox
        return False
    if v in path:                            # v artıq yoldadır
        return False
    return True

def backtrack(graph, path, pos):
    n = len(graph)
    if pos == n:                             # hamısı yerləşib — başa tilov var?
        return graph[path[pos - 1]][path[0]] == 1
    for v in range(1, n):
        if is_safe(graph, path, pos, v):
            path[pos] = v
            if backtrack(graph, path, pos + 1):
                return True
            path[pos] = -1                   # geri al
    return False

graph = [[0,1,0,1,0],[1,0,1,1,1],[0,1,0,0,1],[1,1,0,0,1],[0,1,1,1,0]]
print(hamiltonian_cycle(graph))   # [0, 1, 2, 4, 3, 0]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(N!) (ən pis) |
| Yaddaş | O(N) |

## Nə vaxt
- ✅ Hər node-u dəqiq bir dəfə ziyarət edib başlanğıca qayıtmaq; kiçik qraf (N ≤ 15) və ya yalnız mövcudluq yoxlaması.
- ❌ Böyük qraf (N > 20) — eksponensial; heuristika və ya DP (Held-Karp).
- ❌ Minimum xərcli Hamilton dövrü (TSP) — Held-Karp DP O(2^N × N²) və ya təxminilər.

## Əlaqəli
- [Knight's Tour](KnightsTour.md) — at-gedişi qrafında Hamilton yolu.
- [N-Queens](NQueens.md) — həm də backtracking məhdudiyyət yerləşdirmə.
- [DFS](../GraphAlgorithms/DFS.md) — Hamilton backtracking geri-almalı DFS-dir.
