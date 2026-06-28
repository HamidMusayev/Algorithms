# Counting Sort

**Fikir:** Müqayisə etmədən, hər dəyərin neçə dəfə təkrarlandığını sayırıq, sonra sayğacı oxuyaraq sıralı massivi yenidən qururuq. Müqayisəsiz olduğu üçün O(n log n)-dən sürətli ola bilir.

## Necə işləyir
1. Massivdə ən böyük dəyəri `k` tap.
2. `k + 1` ölçülü, sıfırlarla dolu **sayğac massivi** yarat.
3. **Say:** hər element üçün `count[element]`-i bir artır.
4. (Stabil versiya üçün) sayğacları yığılmış (cumulative) hala gətir — hər dəyər çıxışda öz mövqeyini bilsin.
5. **Çıxış qur:** sayğacı oxuyaraq hər dəyəri lazımi sayda massivə yaz.

## Nümunə
`[4, 2, 2, 8, 3, 3, 1]`

Sayğac: 1→1, 2→2, 3→2, 4→1, 8→1 dəfə
→ `[1, 2, 2, 3, 3, 4, 8]` ✅

## Kod
```python
def counting_sort(arr):
    if not arr:
        return arr
    min_val, max_val = min(arr), max(arr)
    count = [0] * (max_val - min_val + 1)
    for num in arr:
        count[num - min_val] += 1        # mənfi ədədlər üçün min_val ilə sürüşdür
    result = []
    for i, c in enumerate(count):
        result.extend([i + min_val] * c) # hər dəyəri c dəfə yaz
    return result

print(counting_sort([4, 2, 2, 8, 3, 3, 1]))   # [1, 2, 2, 3, 3, 4, 8]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Hər zaman | O(n + k) | O(k) |

`n` = element sayı, `k` = dəyərlərin diapazonu. Stabil ✅ · `k ≫ n` olanda israfçıdır.

## Nə vaxt
- ✅ Dəyərlər tam ədəddir və diapazon `k` kiçikdir (məs. yaşlar 0–120, imtahan balları).
- ✅ Müqayisəli sortun O(n log n) həddini aşmaq lazımdır.
- ✅ Radix Sort daxilində rəqəm-rəqəm istifadə olunur.
- ❌ `k ≫ n` (məs. 10 ədədi 0–10M diapazonunda) — yaddaş israfı.
- ❌ Sürüşən nöqtəli (float) dəyərlər — indeks kimi istifadə oluna bilməz.

## Əlaqəli
- [Radix Sort](RadixSort.md) — böyük tam ədədlər üçün rəqəm-rəqəm Counting Sort istifadə edir.
- [Bucket Sort](BucketSort.md) — həm də müqayisəsiz; vedrələrə paylayır.
- [Merge Sort](MergeSort.md) — diapazon böyük olanda ən yaxşı alternativ.
