# Voronoi Diagram (Voronoi Diaqramı)

**Fikir:** Hovuza bir neçə mənbədən rəngli boya damızdır. Hər rəng bərabər yayılır; iki rəngin görüşdüyü sərhəd hər iki mənbədən bərabər məsafədədir. Voronoi diaqramı məhz bu sərhədlərdir: müstəvini **regionlara** bölür, hər region müəyyən bir "toxum" (site) nöqtəsinə ən yaxın bütün nöqtələri saxlayır. Məsələn: xəstəxana yerləri verildikdə, hər xəstəxananın Voronoi regionu onun ən yaxın olduğu sahədir.

## Necə işləyir (Fortune alqoritmi — sweep line)
1. Soldan-sağa hərəkət edən **süpürən xətt** saxla.
2. **Sahil xətti (beach line)** saxla: parabolik qövslər dəsti (hər qövs site ilə süpürən xəttdən bərabər məsafəli nöqtələri izləyir).
3. İki hadisə tipi: **site hadisəsi** (yeni site → yeni parabola əlavə et) və **dairə hadisəsi** (üç qövs birləşir → Voronoi təpəsi yaranır → ortadakı qövsü çıxar).

> Fortune alqoritmini sıfırdan yazmaq mürəkkəbdir. Praktikada **scipy** və ya həndəsə kitabxanası istifadə et.

## Nümunə
4 site: `(0,0), (4,0), (2,3), (2,-3)` → y=0 xəttinin üstündəki nöqtələr (2,3)-ə ən yaxın; hər region qabarıq çoxbucaqlıdır.

## Kod
```python
import numpy as np
from scipy.spatial import Voronoi

def compute_voronoi(sites):
    """scipy (daxildə Fortune alqoritmi) ilə Voronoi diaqramı."""
    return Voronoi(np.array(sites))

def nearest_site(sites, query_point):
    """query_point-ə ən yaxın site."""
    sites_arr = np.array(sites)
    distances = np.linalg.norm(sites_arr - np.array(query_point), axis=1)
    return sites[np.argmin(distances)]

sites = [(0,0), (4,0), (2,4), (2,-4), (6,2), (-2,2)]
vor = compute_voronoi(sites)
print(vor.vertices)                       # Voronoi təpələri
print(nearest_site(sites, (1, 1)))        # (0, 0)
```

## Mürəkkəblik
| Yanaşma | Vaxt |
|---------|------|
| Sadə (bütün cütlər) | O(n² log n) |
| Fortune sweep line / scipy | O(n log n) |
| Yaddaş | O(n) |

## Nə vaxt
- ✅ Çoxlu sorğu nöqtəsi üçün **ən yaxın site**-i tapmaq; coğrafi xidmət sahəsi; oyun AI ərazi bölgüsü.
- ✅ Delaunay üçbucaqlaması (Voronoi-nin duallı) — mesh yaratma.
- ❌ Tək ən-yaxın-qonşu sorğusu — KD-ağac və ya sadə üsul.
- ❌ Evklid olmayan məsafə (Manhattan) və ya 3D — standart Voronoi uyğun gəlmir.

## Əlaqəli
- [Closest Pair of Points](ClosestPairOfPoints.md) — həm də əsas 2D həndəsə.
- [Convex Hull](ConvexHull.md) — kənar site-ların Voronoi xanaları sonsuzdur; convex hull-a duallı.
- [Sweep Line](SweepLine.md) — Fortune alqoritmi sweep line-dır.
