# Reservoir Sampling (Rezervuar Seçimi)

**Fikir:** Sonsuz paradı izləyirsən və dəqiq 3 lotereya bileti paylamaq istəyirsən ki, hər keçən insanın bilet tutmaq şansı bərabər olsun — amma geri qayıda bilmirsən və paradın nə qədər davam edəcəyini bilmirsən. Reservoir sampling məhz bunu həll edir: `k` elementlik "rezervuar" saxla; hər yeni element gələndə ehtimalla qərar ver, yer qazanırsa, təsadüfi cari sahibi çıxar.

**Mental model:** Hər `i` elementin (1-dən) son rezervuarda olma ehtimalı həmişə dəqiq `k / i`-dir (induksiya ilə sübut olunur).

## Necə işləyir
1. Rezervuarı axının ilk `k` elementi ilə doldur.
2. `i ≥ k` mövqeyindəki hər növbəti element üçün:
   - `[0, i]`-də təsadüfi tam ədəd `j` yarat.
   - `j < k`-dırsa, `reservoir[j]`-i cari elementlə əvəz et.
3. Axın bitəndə rezervuarı qaytar.

> Niyə işləyir: `i` elementi seçilmə ehtimalı `k/(i+1)`-dir; mövcud elementin qalma ehtimalı `i/(i+1)`. Bütün addımlar birlikdə hər elementə `k/n` ehtimal verir.

## Nümunə
Axın `[A,B,C,D,E,F]`, k=3
- A,B,C birbaşa → [A,B,C]
- D: j=rand(0..3), j<3 olsa əvəz et → məs. [A,D,C]
- F: → məs. [A,D,F]
Yekun məs. `[A, D, F]` (hər işlədişdə dəyişir, hamısı bərabər ehtimallı)

## Kod
```python
import random

def reservoir_sample(stream, k):
    """Naməlum uzunluqlu axından bərabər k element seç (Vitter Algorithm R)."""
    reservoir = []
    for i, item in enumerate(stream):
        if i < k:
            reservoir.append(item)            # Faza 1: ilk k elementi doldur
        else:
            j = random.randrange(i + 1)       # [0, i]-də təsadüfi
            if j < k:
                reservoir[j] = item           # yer qazanır, köhnəni çıxarır
    return reservoir

print(reservoir_sample(range(1, 101), 5))     # məs. [73, 12, 55, 8, 91]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n) — bir keçid |
| Yaddaş | O(k) — yalnız rezervuar |

Zəmanət: hər element dəqiq `k/n` ehtimalla seçilir.

## Nə vaxt
- ✅ Ümumi uzunluğu naməlum və ya yaddaşa sığmayan **axından** k təsadüfi element seçmək.
- ✅ Daim gələn data (log faylları, şəbəkə paketləri, sensorlar) — istənilən anda güncəl nümunə.
- ❌ `n` əvvəlcədən məlumdur və O(n) yaddaş var — sadəcə `random.sample()`.
- ❌ Çəkili seçim — çəkili reservoir sampling (Algorithm A-Chao).

## Əlaqəli
- [Monte Carlo / Las Vegas](MonteCarloLasVegas.md) — reservoir sampling Las Vegas tipli təsadüfi alqoritmdir (nəticə həmişə düzgün, vaxt sabit).
