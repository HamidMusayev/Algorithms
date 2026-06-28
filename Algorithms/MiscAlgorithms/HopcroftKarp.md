# Hopcroft-Karp Alqoritmi (İkihissəli Uyğunlaşma)

**Fikir:** İkihissəli (bipartite) qrafda **uyğunlaşma** ortaq ucu olmayan tilov dəstidir. **Maksimum uyğunlaşma** mümkün qədər çox sol node-u sağ node-a cütləşdirir (iş-namizəd təyini, tapşırıq-işçi bölgüsü). Sadə yanaşma O(V×E)-dir. Hopcroft-Karp bir BFS+DFS turunda çoxlu **ən qısa artıran yol** tapır, turların sayını O(V)-dən O(√V)-yə salır. Nəticə: O(E×√V).

## Necə işləyir
**Əsas anlayışlar:** *Sərbəst node* — cütü olmayan node. *Artıran yol* — sərbəst sol node-dan sərbəst sağ node-a, uyğun/qeyri-uyğun tilovların növbələşdiyi yol. Artıran yol əlavə etmək uyğunlaşma ölçüsünü 1 artırır.

1. **BFS fazası:** bütün sərbəst sol node-lardan eyni anda ən qısa artıran yolların qatlı qrafını qur.
2. **DFS fazası:** hər sərbəst sol node üçün qatlı qrafı izləyib artıran yol tap; tilovları çevirib uyğunlaşmanı artır.
3. BFS artıran yol tapmayana qədər təkrarla.

## Nümunə
Sol = {Alice, Bob, Carol, Dave}, Sağ = {A, B, C}
Alice→{A,B}, Bob→{A}, Carol→{B,C}, Dave→{C}
Maksimum uyğunlaşma = 3: Alice→B, Bob→A, Carol/Dave→C.

## Kod
```python
from collections import deque

def hopcroft_karp(adj, left_nodes, right_nodes):
    """adj: {sol_node: [sağ_node-lar]}. (ölçü, sol_uyğunluq, sağ_uyğunluq)."""
    INF = float('inf')
    match_left = {u: None for u in left_nodes}
    match_right = {v: None for v in right_nodes}
    dist = {}

    def bfs():                                  # ən qısa artıran yolların qatlı qrafı
        queue = deque()
        for u in left_nodes:
            dist[u] = 0 if match_left[u] is None else INF
            if match_left[u] is None:
                queue.append(u)
        found = False
        while queue:
            u = queue.popleft()
            for v in adj[u]:
                w = match_right[v]
                if w is None:
                    found = True
                elif dist.get(w, INF) == INF:
                    dist[w] = dist[u] + 1
                    queue.append(w)
        return found

    def dfs(u):                                 # yol boyunca artır
        for v in adj[u]:
            w = match_right[v]
            if w is None or (dist.get(w, INF) == dist[u] + 1 and dfs(w)):
                match_left[u] = v
                match_right[v] = u
                return True
        dist[u] = INF
        return False

    matching = 0
    while bfs():
        for u in left_nodes:
            if match_left[u] is None and dfs(u):
                matching += 1
    return matching, match_left, match_right

adj = {'Alice':['A','B'], 'Bob':['A'], 'Carol':['B','C'], 'Dave':['C']}
size, ml, _ = hopcroft_karp(adj, list(adj), ['A','B','C'])
print(size)   # 3
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(E × √V) |
| Yaddaş | O(V + E) |

## Nə vaxt
- ✅ İki qrupdan elementləri məhdudiyyətlə cütləşdirmək (iş-işçi, tapşırıq-server); maksimum uyğun cüt sayı.
- ✅ Böyük ikihissəli qraflar (E, V minlərlə) — O(E√V) yaxşı miqyaslanır.
- ❌ İkihissəli olmayan uyğunlaşma — Edmonds blossom alqoritmi.
- ❌ Çəkili uyğunlaşma (ümumi çəkini maksimum et) — Macar (Hungarian) alqoritmi.

## Əlaqəli
- [BFS](../GraphAlgorithms/BFS.md) — BFS fazası ən qısa artıran yolları tapır.
- [DFS](../GraphAlgorithms/DFS.md) — DFS fazası qatlı qrafı izləyib artırır.
- [Kruskal](../GraphAlgorithms/Kruskal.md) — qraf məsələləri üçün ağıllı data strukturları (DSU/Union-Find).
