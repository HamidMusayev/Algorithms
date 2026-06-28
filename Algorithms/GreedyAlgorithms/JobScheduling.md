# Job Scheduling (Son Tarixli Tapşırıqların Planlanması)

**Fikir:** Hər biri tam 1 gün çəkən n tapşırıq var; hər birinin mənfəəti və son tarixi var (gün `d`-yə qədər bitməlidir). Gündə yalnız bir tapşırıq. Ümumi mənfəəti maksimum et. Acgöz strategiya: tapşırıqları **mənfəətə görə azalan** sırala; hər birini son tarixindən əvvəlki **ən gec boş günə** yerləşdir (erkən günləri gələcəyə saxla).

## Necə işləyir
1. Tapşırıqları mənfəətə görə **azalan** sırala.
2. `max_deadline + 1` ölçülü `slots` massivi (hamısı boş).
3. Hər tapşırıq üçün (mənfəət sırası ilə):
   - Son tarix ≤ olan **ən gec boş günü** tap.
   - Tapılsa: tapşırığı ora yerləşdir, mənfəət əlavə et.
   - Tapılmasa: atla.

## Nümunə
Tapşırıqlar: a(dl=2, $100), b(dl=1, $19), c(dl=2, $27), d(dl=1, $25), e(dl=3, $15)
- a → gün 2; c → gün 1; d, b → boş gün yox, atla; e → gün 3
- Seçildi: **c, a, e** — ümumi **$142** ✅

## Kod
```python
def job_scheduling(jobs):
    """jobs: (id, son_tarix, mənfəət)."""
    jobs = sorted(jobs, key=lambda x: x[2], reverse=True)   # mənfəətə görə azalan
    max_deadline = max(j[1] for j in jobs)
    slots = [None] * (max_deadline + 1)
    total = 0
    for jid, dl, profit in jobs:
        for t in range(min(dl, max_deadline), 0, -1):       # ən gec boş günü tap
            if slots[t] is None:
                slots[t] = jid
                total += profit
                break
    selected = [(t, slots[t]) for t in range(1, max_deadline + 1) if slots[t]]
    return selected, total

jobs = [('a',2,100), ('b',1,19), ('c',2,27), ('d',1,25), ('e',3,15)]
print(job_scheduling(jobs))   # ([(1,'c'),(2,'a'),(3,'e')], 142)
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n log n + n × d) (sadə axtarış) |
| Yaddaş | O(d) |

> Union-Find (DSU) ilə "ən gec boş gün" axtarışı demək olar O(1) olur → ümumi O(n log n).

## Nə vaxt
- ✅ Hər biri **bir vahid vaxt** çəkən, son tarixli, mənfəətli tapşırıqlar.
- ✅ CPU tapşırıq planlaşdırması (dövrdə bir proses).
- ❌ Tapşırıqlar müxtəlif müddətli — greedy işləmir.
- ❌ Tapşırıq sayını (mənfəəti yox) maksimum etmək — Activity Selection.

## Əlaqəli
- [Activity Selection](ActivitySelection.md) — üst-üstə düşməyən fəaliyyət sayını maksimum edir.
- [Fractional Knapsack](FractionalKnapsack.md) — həm də greedy: nisbətə görə seçim.
