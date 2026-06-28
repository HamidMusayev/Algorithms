# Kruskal Alqoritmi

**Fikir:** Bütün mümkün yolları (tilovları) qiymətə görə sıralayırıq və ən ucuzdan başlayıb əlavə edirik. İki şəhər artıq qoşulubsa, o yolu atlayırıq (dövr olardı). Hansı şəhərlərin qoşulu olduğunu **Union-Find** ilə izləyirik. Nəticə: **Minimal Örtən Ağac (MST)**.

## Necə işləyir
1. Hər node öz dəstində olan **DSU (ayrıq dəstlər)** yarat.
2. Bütün tilovları çəkiyə görə artan sırala.
3. Hər `(u, v, w)` tilovu üçün ucuzdan bahaya:
   - `find(u) ≠ find(v)` → fərqli komponentlər → **təhlükəsiz əlavə et**, birləşdir, çəkini topla.
   - `find(u) == find(v)` → dövr olardı → atla.
4. MST-də V−1 tilov olanda dayan.

## Nümunə
Sıralı tilovlar: `(0,1,1), (1,2,1), (2,3,1), (0,2,3), (1,3,4)`
- İlk üçü əlavə olunur, son ikisi dövr yaradır → atlanır.
- MST = {0-1, 1-2, 2-3}, ümumi çəki = **3** ✅

## Kod
```python
class DSU:
    """Yol sıxılması və ranqa görə birləşmə ilə ayrıq dəstlər."""
    def __init__(self, n):
        self.parent = list(range(n))
        self.rank = [0] * n
    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])   # yol sıxılması
        return self.parent[x]
    def union(self, x, y):
        rx, ry = self.find(x), self.find(y)
        if rx == ry:
            return False                  # eyni komponent → dövr
        if self.rank[rx] < self.rank[ry]:
            rx, ry = ry, rx
        self.parent[ry] = rx
        if self.rank[rx] == self.rank[ry]:
            self.rank[rx] += 1
        return True

def kruskal(V, edges):
    edges.sort(key=lambda e: e[2])        # çəkiyə görə sırala
    dsu = DSU(V)
    mst, total = [], 0
    for u, v, w in edges:
        if dsu.union(u, v):               # dövr yaratmırsa
            mst.append((u, v, w))
            total += w
            if len(mst) == V - 1:
                break
    return mst, total

edges = [(0,1,1), (0,2,3), (1,2,1), (1,3,4), (2,3,1)]
print(kruskal(4, edges))   # ([(0,1,1),(1,2,1),(2,3,1)], 3)
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(E log E) (sıralama üstünlük təşkil edir) |
| Yaddaş | O(V) |

Union-Find əməliyyatları (yol sıxılması + ranq ilə) demək olar O(1)-dir.

## Nə vaxt
- ✅ Bütün node-ları minimum ümumi çəki ilə qoşmaq; qraf **tilov siyahısı** kimi verilib.
- ✅ Qraf seyrəkdir (E node sayına yaxın).
- ❌ Sıx qraf (E ≈ V²) — Prim daha səmərəli.
- ❌ Yönlü qraf — minimal örtən arborescensiya lazımdır.

## Əlaqəli
- [Prim](Prim.md) — həm də MST tapır; bütün tilovları sıralamaq yox, bir node-dan böyüyür.
- [Dijkstra](Dijkstra.md) — ən qısa yol, MST yox; fərqli məqsəd.
