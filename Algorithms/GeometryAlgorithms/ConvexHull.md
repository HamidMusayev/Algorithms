# Convex Hull (Qabarıq Örtük)

**Fikir:** Taxtaya mıxlar çalıb, hamısının ətrafına rezin halqa keçirib buraxsan, halqa ən kənar mıxlara toxunub hamısını əhatə edən ən kiçik qabarıq fiqur yaradır — bu, **qabarıq örtükdür**. Yəni: 2D nöqtələr çoxluğunun, hər nöqtəni daxilində və ya sərhədində saxlayan ən kiçik qabarıq çoxbucaqlısı.

**Variantlar:** Graham's Scan (O(n log n)), Jarvis March (O(n·h)), Andrew's Monotone Chain (O(n log n) — ən sadə, aşağıda).

## Necə işləyir (Andrew's Monotone Chain)
1. Bütün nöqtələri (x, y)-ə görə sırala.
2. **Aşağı örtük:** soldan-sağa gəz. Hər yeni nöqtə üçün son iki örtük nöqtəsi ilə **vektor hasilinə** bax; sağa dönüş (və ya kollinear)-dırsa, sonuncunu çıxar.
3. **Yuxarı örtük:** sağdan-sola eyni məntiqlə.
4. İki yarını birləşdir (ortaq uçları çıxararaq) — saat əqrəbinin əksinə (CCW) sıra.

**Vektor hasili:** O, A, B üçün `(A-O) × (B-O)` OAB üçbucağının işarəli sahəsidir:
- Müsbət → B, OA-nın solunda (CCW dönüş) — saxla.
- Mənfi → B, OA-nın sağında (CW dönüş) — A-nı çıxar.
- Sıfır → kollinear.

## Nümunə
Nöqtələr: `(0,0),(1,1),(2,2),(3,1),(4,0),(2,-1),(1,0)`
→ örtük: `(0,0) → (2,-1) → (4,0) → (3,1) → (2,2)`; daxili (1,0) örtükdə yoxdur.

## Kod
```python
def convex_hull(points):
    points = sorted(set(map(tuple, points)))
    if len(points) <= 1:
        return points

    def cross(O, A, B):                       # müsbət=CCW, mənfi=CW, 0=kollinear
        return (A[0]-O[0]) * (B[1]-O[1]) - (A[1]-O[1]) * (B[0]-O[0])

    lower = []                                # aşağı örtük
    for p in points:
        while len(lower) >= 2 and cross(lower[-2], lower[-1], p) <= 0:
            lower.pop()
        lower.append(p)

    upper = []                                # yuxarı örtük
    for p in reversed(points):
        while len(upper) >= 2 and cross(upper[-2], upper[-1], p) <= 0:
            upper.pop()
        upper.append(p)

    return lower[:-1] + upper[:-1]            # ortaq uçları çıxar (CCW)

pts = [(0,0),(1,1),(2,2),(3,1),(4,0),(2,-1),(1,0)]
print(convex_hull(pts))   # [(0,0),(2,-1),(4,0),(3,1),(2,2)]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt (Monotone Chain / Graham) | O(n log n) |
| Vaxt (Jarvis March) | O(n·h), h = örtük ölçüsü |
| Yaddaş | O(n) |

## Nə vaxt
- ✅ Nöqtə buludunun ən kənar sərhədi; toqquşma aşkarlama (qabarıq obyektlər); coğrafi sərhəd.
- ✅ Örtük ölçüsü h ≪ n — Jarvis March O(n·h) sürətli ola bilər.
- ❌ **Çökük (concave)** sərhəd lazımdır — alpha shapes / concave hull.
- ❌ 3D örtük — 2D alqoritmləri ümumiləşmir (Quickhull 3D).

## Əlaqəli
- [Line Intersection](LineIntersection.md) — dönüş istiqamətini vektor hasili ilə yoxlayır.
- [Closest Pair of Points](ClosestPairOfPoints.md) — əsas 2D həndəsə məsələsi.
- [Voronoi Diagram](VoronoiDiagram.md) — Delaunay üçbucaqlaması ilə əlaqəli.
