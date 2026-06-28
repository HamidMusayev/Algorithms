# Line Intersection (Xətt Parçalarının Kəsişməsi)

**Fikir:** İki parça kəsişir ya yox. Əsas alət **vektor hasili / oriyentasiya testidir**: O, A, B üçün OA-dan OB-yə dönüş saat əqrəbi, əksi və ya kollineardır? AB və CD parçaları yalnız o halda kəsişir ki: A və B CD xəttinin **əks tərəflərində**, VƏ C və D AB xəttinin **əks tərəflərində** olsun.

## Necə işləyir
**Üç nöqtənin oriyentasiyası:**
`cross = (A.x-O.x)(B.y-O.y) - (A.y-O.y)(B.x-O.x)` → >0 CCW, <0 CW, =0 kollinear.

**P1P2 və P3P4 üçün kəsişmə testi:**
1. `o1=orient(P1,P2,P3)`, `o2=orient(P1,P2,P4)`, `o3=orient(P3,P4,P1)`, `o4=orient(P3,P4,P2)`.
2. **Ümumi hal:** `o1 ≠ o2` VƏ `o3 ≠ o4` → kəsişir.
3. **Kollinear xüsusi hallar:** oriyentasiya 0-dırsa, nöqtənin parçada olub-olmadığını yoxla.

## Nümunə
- (1,1)-(10,1) və (1,2)-(10,2) (paralel) → o1==o2 → **kəsişmir**
- (0,0)-(10,10) və (0,10)-(10,0) (diaqonallar) → o1≠o2, o3≠o4 → **(5,5)-də kəsişir** ✅

## Kod
```python
def orient(p, q, r):
    val = (q[0]-p[0])*(r[1]-p[1]) - (q[1]-p[1])*(r[0]-p[0])
    return (val > 0) - (val < 0)              # 1=CCW, -1=CW, 0=kollinear

def on_segment(p, q, r):
    return (min(p[0],r[0]) <= q[0] <= max(p[0],r[0]) and
            min(p[1],r[1]) <= q[1] <= max(p[1],r[1]))

def segments_intersect(p1, p2, p3, p4):
    o1, o2 = orient(p1,p2,p3), orient(p1,p2,p4)
    o3, o4 = orient(p3,p4,p1), orient(p3,p4,p2)
    if o1 != o2 and o3 != o4:                 # ümumi hal
        return True
    if o1 == 0 and on_segment(p1, p3, p2): return True   # kollinear hallar
    if o2 == 0 and on_segment(p1, p4, p2): return True
    if o3 == 0 and on_segment(p3, p1, p4): return True
    if o4 == 0 and on_segment(p3, p2, p4): return True
    return False

print(segments_intersect((1,1),(10,1),(1,2),(10,2)))     # False (paralel)
print(segments_intersect((0,0),(10,10),(0,10),(10,0)))    # True
```

## Mürəkkəblik
| Əməliyyat | Vaxt |
|-----------|------|
| Tək cüt kəsişmə testi | O(1) |
| n parça, bütün kəsişmələr (Bentley-Ottmann) | O((n + k) log n) |

## Nə vaxt
- ✅ İki parçanın kəsişib-kəsişmədiyini yoxlamaq; çoxbucaqlı toqquşma, şüa atma (ray casting).
- ✅ Convex hull-da sol/sağ dönüş yoxlaması.
- ❌ n parçada bütün kəsişmələr — Bentley-Ottmann sweep line.

## Əlaqəli
- [Convex Hull](ConvexHull.md) — dönüş aşkarlama üçün oriyentasiya/vektor hasili istifadə edir.
- [Sweep Line](SweepLine.md) — Bentley-Ottmann kəsişmə testini alt-prosedur kimi işlədir.
- [Closest Pair of Points](ClosestPairOfPoints.md) — həm də əsas hesablama həndəsəsi.
