# Monte Carlo və Las Vegas Alqoritmləri

**Fikir:** İki fərqli təsadüfi alqoritm növü:

- **Monte Carlo:** sürətli işləyir, kiçik səhv ehtimalını qəbul edir. Məs. seçim nəticəsini təxmin etmək üçün 1000 təsadüfi seçici sorğusu — sürətli, amma bəzən səhv.
- **Las Vegas:** vaxt qeyri-müəyyən ola bilər, amma cavab HƏMİŞƏ düzgündür. Məs. dəsti təsadüfi qarışdır, sıralı olub-olmadığını yoxla, təsdiqlənənə qədər təkrarla.

**Mental model:** Monte Carlo = "sürətli, amma səhv ola bilər (məhdud ehtimalla)". Las Vegas = "həmişə düzgün, amma vaxt təsadüfi".

## Necə işləyir
**Monte Carlo:** təsadüfiliklə cavabı təxmin et, sübut olunan səhv hədləri ilə. Nümunələr: π təxmini, Miller-Rabin, ML-də random forests/SGD. k müstəqil tur işlədsən, hamısında səhv olma ehtimalı `εᵏ` (eksponensial kiçik).

**Las Vegas:** axtarışı təsadüfiliklə yönəlt, amma qəbul etməzdən əvvəl yoxla. Nümunələr: təsadüfi pivotlu QuickSort (O(n log n) gözlənilən, həmişə düzgün), təsadüfi backtracking.

## Nümunə
**Monte Carlo — π təxmini:** [0,1]² kvadratında 1M təsadüfi nöqtə at, vahid dairə içindəkiləri (x²+y²≤1) say. Nisbət ≈ π/4 → π ≈ 4·(içəri/cəm). 1M nöqtə ilə səhv adətən < 0.001.

**Las Vegas — QuickSort:** təsadüfi pivot seç, bölüşdür. Həmişə düzgün, girişdən asılı olmayaraq gözlənilən O(n log n).

## Kod
```python
import random

def estimate_pi(num_samples=1_000_000):
    """Monte Carlo π təxmini. Səhv ≈ O(1/√n)."""
    inside = 0
    for _ in range(num_samples):
        x, y = random.random(), random.random()
        if x*x + y*y <= 1.0:
            inside += 1
    return 4 * inside / num_samples

def quicksort_lv(arr):
    """Las Vegas QuickSort. Həmişə düzgün, gözlənilən O(n log n)."""
    if len(arr) <= 1:
        return arr
    pivot = random.choice(arr)                # təsadüfi pivot — Las Vegas açarı
    left = [x for x in arr if x < pivot]
    mid = [x for x in arr if x == pivot]
    right = [x for x in arr if x > pivot]
    return quicksort_lv(left) + mid + quicksort_lv(right)

print(f"π ≈ {estimate_pi(1_000_000):.5f}")    # ≈ 3.14159
print(quicksort_lv([5,2,8,1,9,3,7,4,6]))      # [1,2,3,4,5,6,7,8,9]
```

## Mürəkkəblik
| Növ | Düzgünlük | Vaxt |
|-----|-----------|------|
| Monte Carlo | Ehtimallı (ε səhv) | Sabit |
| Las Vegas | Həmişə düzgün | Təsadüfi (gözlənilən) |

## Nə vaxt
- ✅ **Monte Carlo:** dəqiq hesablama qeyri-mümkün, təxmin qəbuledilən (ədədi inteqrasiya, böyük ədəd ilkinliyi).
- ✅ **Las Vegas:** dəqiq cavab lazımdır, amma təsadüfilik sürətləndirir (təsadüfi pivot pis halı önləyir).
- ❌ Monte Carlo: dəqiq cavab lazımdır, təxmin olmaz.
- ❌ Las Vegas: sabit son tarix var (təkrar etmək olmaz).

## Əlaqəli
- [Miller-Rabin](../NumberTheoryAlgorithms/MillerRabin.md) — klassik Monte Carlo ilkinlik testi.
- [Karger's Algorithm](KargerAlgorithm.md) — Monte Carlo min-kəsim.
- [Reservoir Sampling](ReservoirSampling.md) — Las Vegas tipli: həmişə bərabər düzgün.
- [Quick Sort](../SortAlgorithms/QuickSort.md) — təsadüfi pivot onu Las Vegas edir.
