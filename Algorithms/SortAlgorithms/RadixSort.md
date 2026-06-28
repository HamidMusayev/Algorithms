# Radix Sort

**Fikir:** Ədədləri tam müqayisə etmək əvəzinə **rəqəm-rəqəm** sıralayırıq — əvvəlcə təklik rəqəminə, sonra onluğa, sonra yüzlüyə görə. Hər keçiddə stabil sort (Counting Sort) istifadə edilir.

## Necə işləyir
1. Ən böyük ədədi tap — neçə rəqəm keçidi lazım olduğunu bil.
2. Hər rəqəm mövqeyi üçün (təklik, onluq, yüzlük, ...):
   - O rəqəmə görə massivi **Counting Sort** (stabil!) ilə sırala.
3. Bütün keçidlərdən sonra massiv tam sıralıdır.

> Açar fikir: Counting Sort stabil olduğu üçün əvvəlki keçidlərin sırası qorunur. Beləliklə təklikdən sonra onluğa görə sıralayanda eyni onluğa malik ədədlər təklik sırasını saxlayır.

## Nümunə
`[170, 45, 75, 90, 802, 24, 2, 66]`
- Təklik → `[170, 90, 802, 2, 24, 45, 75, 66]`
- Onluq → `[802, 2, 24, 45, 66, 170, 75, 90]`
- Yüzlük → `[2, 24, 45, 66, 75, 90, 170, 802]` ✅

## Kod
```python
def counting_sort_by_digit(arr, exp):
    n = len(arr)
    output = [0] * n
    count = [0] * 10
    for num in arr:                       # bu mövqedəki rəqəmləri say
        count[(num // exp) % 10] += 1
    for i in range(1, 10):                # yığılmış hala gətir
        count[i] += count[i - 1]
    for num in reversed(arr):             # stabillik üçün tərsinə
        d = (num // exp) % 10
        count[d] -= 1
        output[count[d]] = num
    arr[:] = output

def radix_sort(arr):
    if not arr:
        return arr
    max_val = max(arr)
    exp = 1
    while max_val // exp > 0:             # hər rəqəm mövqeyi üçün
        counting_sort_by_digit(arr, exp)
        exp *= 10
    return arr

print(radix_sort([170, 45, 75, 90, 802, 24, 2, 66]))
# [2, 24, 45, 66, 75, 90, 170, 802]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Hər zaman | O(n × d) | O(n + k) |

`d` = ən böyük ədədin rəqəm sayı, `k` = baza (onluq üçün 10). Stabil ✅ · `d` kiçik olanda müqayisəli sortları üstələyir.

## Nə vaxt
- ✅ Az rəqəmli böyük tam ədədlər; sabit uzunluqlu sətirlər (IP, tarix, DNA).
- ✅ O(n log n) həddini aşmaq lazım olanda.
- ❌ Çox geniş diapazon (çox rəqəm) — keçidlər bahalaşır.
- ❌ Kiçik n (< 100) və ya float ədədlər — QuickSort daha uyğundur.

## Əlaqəli
- [Counting Sort](CountingSort.md) — hər rəqəm keçidi üçün alt-prosedurdur.
- [Bucket Sort](BucketSort.md) — həm də müqayisəsiz; dəyər diapazonuna görə paylayır.
- [Merge Sort](MergeSort.md) — radix uyğun gəlməyəndə ən yaxşı alternativ.
