# Bucket Sort

**Fikir:** Elementləri diapazona görə "vedrələrə" paylayırıq, hər kiçik vedrəni ucuz şəkildə sıralayırıq, sonra vedrələri sırayla birləşdiririk.

## Necə işləyir
1. Giriş dəyərlərinin diapazonunu müəyyən et.
2. Hər biri bərabər alt-diapazonu əhatə edən `k` boş vedrə yarat.
3. **Payla:** hər elementi dəyərinə görə uyğun vedrəyə qoy.
4. **Hər vedrəni** ayrıca sırala (adətən Insertion Sort — vedrələr kiçikdir).
5. Bütün vedrələri sırayla **birləşdir**.

## Nümunə
`[0.42, 0.32, 0.23, 0.52, 0.25, 0.47]` (6 vedrə)
- Vedrə 1: `[0.23, 0.25, 0.32]`
- Vedrə 2: `[0.42, 0.47]`
- Vedrə 3: `[0.52]`
- Birləşdir → `[0.23, 0.25, 0.32, 0.42, 0.47, 0.52]` ✅

## Kod
```python
def bucket_sort(arr):
    """[0, 1) aralığındakı float ədədləri sıralayır."""
    if not arr:
        return arr
    n = len(arr)
    buckets = [[] for _ in range(n)]
    for num in arr:
        index = min(int(num * n), n - 1)   # dəyəri vedrə indeksinə çevir
        buckets[index].append(num)
    for bucket in buckets:
        bucket.sort()                       # hər vedrəni sırala
    return [num for bucket in buckets for num in bucket]

print(bucket_sort([0.42, 0.32, 0.23, 0.52, 0.25, 0.47]))
# [0.23, 0.25, 0.32, 0.42, 0.47, 0.52]
```

> Tam ədədlər üçün dəyəri `(num - min) / (max - min + 1)` ilə [0,1)-ə normallaşdır.

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Orta (bərabər paylanmış) | O(n + k) | O(n + k) |
| Ən pis (hamısı bir vedrədə) | O(n²) | O(n + k) |

Stabil ✅ · `k` = vedrə sayı.

## Nə vaxt
- ✅ Məlum diapazonda və **bərabər paylanmış** float ədədlər.
- ✅ Düzgün data üçün O(n log n) həddini aşmaq lazımdır.
- ❌ Data bərabər paylanmayıb — hamısı bir vedrəyə yığılır → O(n²).
- ❌ Tam ədəd, kiçik diapazon — Counting Sort daha sadə və zəmanətlidir.

## Əlaqəli
- [Counting Sort](CountingSort.md) — oxşar fikir, amma diapazon yox, dəqiq dəyərləri sayır.
- [Radix Sort](RadixSort.md) — həm də müqayisəsiz; rəqəm-rəqəm işləyir.
- [Insertion Sort](InsertionSort.md) — adətən hər vedrənin içində istifadə olunur.
