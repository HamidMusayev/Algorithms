# PageRank Alqoritmi

**Fikir:** Təsadüfi veb-istifadəçi: təsadüfi səhifədən başlayır, təsadüfi linklərə klikləyir, bəzən (1−d ehtimalla) darıxıb tamamilə təsadüfi səhifəyə tullanır. Çox uzun müddətdən sonra istifadəçinin müəyyən səhifədə olma ehtimalı o səhifənin **PageRank**-ı — əhəmiyyətidir. Çoxlu əhəmiyyətli səhifədən link alan səhifələr yüksək bal alır.

**Mental model:** Əhəmiyyətli səhifələrə digər əhəmiyyətli səhifələr işarə edir. Təsadüfi gəzişmə simulyasiyası işlət — istifadəçi vaxtının çoxunu harada keçirirsə, o səhifənin balıdır.

## Necə işləyir
**Düstur:** `PR(u) = (1−d)/N + d · Σ (PR(v) / çıxış_dərəcəsi(v))` (u-ya link verən bütün v-lər üzrə).
- `d` = sönmə əmsalı (adətən 0.85) — link izləmə ehtimalı.
- `1−d` = təsadüfi tullanma ehtimalı. `N` = ümumi səhifə sayı.

**Alqoritm (Güc İterasiyası):** bütün PR-ı `1/N` başlat; düsturu təkrar tətbiq et; dəyişmələr ε-dan kiçik olanda dayan.

## Nümunə
A→B, A→C, B→C, C→A, D→C; d=0.85, N=4
Bir neçə iterasiyadan sonra **C** ən yüksək balı alır (A, B və D ondan link verir), D ən aşağı (gələn link yoxdur).

## Kod
```python
def pagerank(graph, d=0.85, epsilon=1e-8, max_iter=100):
    """graph: {node: [link verdiyi node-lar]}; {node: bal} qaytarır."""
    nodes = list(graph.keys())
    N = len(nodes)
    pr = {node: 1.0 / N for node in nodes}
    in_links = {node: [] for node in nodes}
    out_degree = {node: len(graph[node]) for node in nodes}
    for node in nodes:
        for target in graph[node]:
            in_links[target].append(node)
    teleport = (1 - d) / N
    for _ in range(max_iter):
        new_pr = {}
        for node in nodes:
            link_sum = sum(pr[v] / out_degree[v]
                           for v in in_links[node] if out_degree[v] > 0)
            new_pr[node] = teleport + d * link_sum
        change = sum(abs(new_pr[v] - pr[v]) for v in nodes)
        pr = new_pr
        if change < epsilon:
            break
    total = sum(pr.values())
    return {node: score / total for node, score in pr.items()}   # normallaşdır

graph = {'A': ['B','C'], 'B': ['C'], 'C': ['A'], 'D': ['C']}
for page, score in sorted(pagerank(graph).items(), key=lambda x: -x[1]):
    print(f"{page}: {score:.4f}")   # C ən yüksək
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Hər iterasiya | O(V + E) |
| Ümumi | O(k × (V + E)) |
| Yaddaş | O(V) |

Adətən ε=10⁻⁸ üçün 20–100 iterasiya.

## Nə vaxt
- ✅ Yönlü qrafda node-ları əhəmiyyətinə görə sıralamaq; əhəmiyyət sənə kimin link verdiyindən asılı olanda.
- ✅ Veb səhifə sıralaması, sosial şəbəkə təsiri, sitat şəbəkəsi analizi, tövsiyə sistemləri.
- ❌ Çəkisiz, yönsüz qraflar — daha sadə mərkəzilik ölçüləri.
- ❌ Real-vaxt sıralama (tez-tez yenilənmə) — PageRank-ı yenidən hesablamaq bahalıdır.

## Əlaqəli
- [BFS](../GraphAlgorithms/BFS.md) · [DFS](../GraphAlgorithms/DFS.md) — qraf strukturunu gəzir; fərqli məqsədlər.
- [Monte Carlo / Las Vegas](MonteCarloLasVegas.md) — PageRank təsadüfi gəzişməni simulyasiya edir (Monte Carlo təfsiri).
