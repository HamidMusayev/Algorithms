# Karger Alqoritmi (Minimum Kəsim)

**Fikir:** Qrafın **minimum kəsimi** silinməsi qrafı iki yerə ayıran ən kiçik tilov dəstidir — şəbəkənin ən zəif həlqəsi. Karger təsadüfi yanaşma: dəfələrlə təsadüfi tilov seçib onu **büz** (iki ucunu bir supernode-a birləşdir, qalan tilovları saxla, öz-halqaları sil). V−2 büzülmədən sonra dəqiq 2 supernode qalır; aralarındakı tilovlar bir kəsim təşkil edir. Çox dəfə təkrarla, minimumu götür.

**Mental model:** Node-ları təsadüfi birləşdir, 2 qalana qədər. Qalan tilovlar bir kəsimdir. Çox dəfə et; görülən ən kiçik kəsim çox güman əsl minimumdur.

## Necə işləyir
**Bir işlədiş:**
1. Orijinal qrafdan başla.
2. 2-dən çox supernode qaldıqca:
   - **Təsadüfi tilov** `(u, v)` seç.
   - **Büz:** v-ni u-ya birləşdir; v-yə gedən bütün tilovlar indi u-ya gedir; öz-halqaları sil.
3. Qalan 2 supernode arasındakı tilovları say — kəsim ölçüsü budur.

**Düzgünlük üçün çox işlədiş:** alqoritmi çox dəfə işlət (O(V² log V)), tapılan minimum kəsimi qaytar.

## Nümunə
4 node `{1,2,3,4}`, tilovlar: `1-2, 2-3, 3-4, 4-1, 1-3`
- 1-3 büz → {1,3} ... → 4 büz → {1,3,4}
- Qalan: {1,3,4} və {2}, aralarında 2 tilov → kəsim = 2 ✅

## Kod
```python
import random, copy, math

def karger_one_run(graph):
    """graph: {node: [qonşular]}, çoxlu tilovlar təkrar kimi. Kəsim ölçüsünü qaytarır."""
    g = copy.deepcopy(graph)
    while len(g) > 2:
        u = random.choice(list(g.keys()))
        v = random.choice(g[u])
        g[u].extend(g[v])                        # v-ni u-ya büz
        for neighbor in g[v]:
            g[neighbor] = [u if x == v else x for x in g[neighbor]]
        g[u] = [x for x in g[u] if x != u]       # öz-halqaları sil
        del g[v]
    return len(g[list(g.keys())[0]])

def min_cut(graph, num_trials=None):
    n = len(graph)
    if num_trials is None:
        num_trials = max(100, n * n * int(math.log(n) + 1))
    return min(karger_one_run(graph) for _ in range(num_trials))

graph = {1: [2,3,4], 2: [1,3], 3: [1,2,4], 4: [1,3]}
print(min_cut(graph, num_trials=200))   # 2
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Bir işlədiş | O(V²) |
| İşlədiş başına uğur ehtimalı | ≥ 2 / (V·(V−1)) |
| Yüksək əminlik üçün işlədiş | O(V² log V) |

## Nə vaxt
- ✅ Yönsüz qrafda minimum kəsim; şəbəkə etibarlılığı; şəkil seqmentasiyası (qraf kimi modelləşir).
- ❌ Determinik nəticə lazımdır — Stoer-Wagner O(V³) və ya maks-axın.
- ❌ Yönlü qraflar — maks-axın/min-kəsim (Ford-Fulkerson).
- ❌ Böyük qraflar (V > 1000) — Karger-Stein variantı olmadan yavaş.

## Əlaqəli
- [Monte Carlo / Las Vegas](MonteCarloLasVegas.md) — Karger Monte Carlo alqoritmidir (işlədiş başına səhv verə bilər).
- [Floyd's Cycle Detection](FloydCycleDetection.md) — həm də təsadüfi/ehtimallı alqoritm.
