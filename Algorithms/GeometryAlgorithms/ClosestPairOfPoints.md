# Closest Pair of Points (Ən Yaxın Nöqtə Cütü)

**Fikir:** n nöqtə içində bir-birinə ən yaxın iki nöqtəni tap. Sadə üsul hər cütü yoxlayır: O(n²) — böyük n üçün yavaş. Böl-və-idarə et: nöqtələri sol/sağ yarıya böl, hər yarıda ən yaxın cütü rekursiv tap, sonra yalnız bölmə xəttinə yaxın nöqtələri ("zolaq") yoxla. Açar fikir: zolaqda hər nöqtə həndəsi səbəbdən yalnız növbəti 7 nöqtə ilə müqayisə olunur — zolaq yoxlaması O(n)-dir.

## Necə işləyir
1. Bütün nöqtələri x-ə görə **sırala**.
2. Median x-də **böl**: sol və sağ yarı.
3. **İdarə et:** hər yarıda min məsafə `dL`, `dR` tap; `d = min(dL, dR)`.
4. **Birləşdir:** **zolağı** yoxla — bölmə xəttindən `d` məsafəsindəki nöqtələr:
   - Zolaq nöqtələrini y-ə görə sırala.
   - Hər zolaq nöqtəsini növbəti 7 ilə müqayisə et; `d`-dən yaxındırsa, `d`-ni yenilə.

> Niyə yalnız 7? `d × 2d` düzbucaqlıda cüt məsafəsi ≥ d olan ən çox 8 nöqtə yerləşə bilər.

## Nümunə
`(2,3),(12,30),(40,50),(5,1),(12,10),(3,4)`
→ ən yaxın cüt: `(2,3)` və `(3,4)`, məsafə ≈ 1.41

## Kod
```python
import math

def dist(p, q):
    return math.hypot(p[0] - q[0], p[1] - q[1])

def brute_force(pts):                          # baza: kiçik dəstlər
    best = (float('inf'), None)
    for i in range(len(pts)):
        for j in range(i + 1, len(pts)):
            d = dist(pts[i], pts[j])
            if d < best[0]:
                best = (d, (pts[i], pts[j]))
    return best

def closest_pair(points):
    points = sorted(points)                    # x-ə görə bir dəfə sırala
    def rec(pts):
        if len(pts) <= 3:
            return brute_force(pts)
        mid = len(pts) // 2
        mid_x = pts[mid][0]
        d, pair = min(rec(pts[:mid]), rec(pts[mid:]))
        strip = sorted([p for p in pts if abs(p[0] - mid_x) < d], key=lambda p: p[1])
        for i in range(len(strip)):
            for j in range(i + 1, min(i + 8, len(strip))):
                if strip[j][1] - strip[i][1] >= d:
                    break
                nd = dist(strip[i], strip[j])
                if nd < d:
                    d, pair = nd, (strip[i], strip[j])
        return d, pair
    return rec(points)

print(closest_pair([(2,3),(12,30),(40,50),(5,1),(12,10),(3,4)]))
# (1.414..., ((2,3),(3,4)))
```

## Mürəkkəblik
| Yanaşma | Vaxt |
|---------|------|
| Sadə üsul | O(n²) |
| Böl-və-idarə et | O(n log n) |
| Yaddaş | O(n) |

## Nə vaxt
- ✅ 2D nöqtələr çoxluğunda ən yaxın iki elementi tapmaq; böyük n (> 1000).
- ✅ Klasterləşmə, məkan analizi.
- ❌ Kiçik n (< 100) — sadə O(n²) daha asandır.
- ❌ 3D və ya yuxarı — alqoritm genişlənir, amma mürəkkəbləşir.

## Əlaqəli
- [Convex Hull](ConvexHull.md) — həm də əsas həndəsə; fərqli məsələ.
- [Line Intersection](LineIntersection.md) — başqa əsas hesablama həndəsəsi primitivi.
