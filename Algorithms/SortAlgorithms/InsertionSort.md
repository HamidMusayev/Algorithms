# Insertion Sort

**Fikir:** Kart oynayan kimi: hər yeni elementi götürüb sol tərəfdəki artıq sıralı hissədə öz yerinə "soxuruq".

## Necə işləyir
1. İkinci elementdən başla (birinci tək-tək onsuz da sıralıdır).
2. Cari elementi `key` kimi saxla.
3. `key`-i solundakı elementlərlə sağdan-sola müqayisə et.
4. `key`-dən böyük olan hər elementi bir addım sağa sürüşdür.
5. `key`-dən kiçik (və ya bərabər) elementə çatanda `key`-i boşluğa yerləşdir.
6. Növbəti elementə keç və təkrarla.

## Nümunə
`[12, 11, 13, 5, 6]`
- key=11 → `[11, 12, 13, 5, 6]`
- key=13 → dəyişməz
- key=5 → `[5, 11, 12, 13, 6]`
- key=6 → `[5, 6, 11, 12, 13]` ✅

## Kod
```python
def insertion_sort(arr):
    for i in range(1, len(arr)):
        key = arr[i]        # sıralı hissəyə soxulacaq element
        j = i - 1
        # key-dən böyük elementləri bir addım sağa sürüşdür
        while j >= 0 and arr[j] > key:
            arr[j + 1] = arr[j]
            j -= 1
        arr[j + 1] = key    # key-i öz yerinə qoy
    return arr

print(insertion_sort([12, 11, 13, 5, 6]))   # [5, 6, 11, 12, 13]
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Ən yaxşı (sıralı) | O(n) | O(1) |
| Orta / Ən pis | O(n²) | O(1) |

Stabil ✅ · Yerində ✅ · Adaptiv (sıralıya nə qədər yaxındırsa, o qədər sürətli).

## Nə vaxt
- ✅ Kiçik (n < 20) və ya sıralıya yaxın massivlər.
- ✅ Data axın şəklində gəlir və hər an sıralı qalmalıdır (online sorting).
- ✅ Timsort (Python-un daxili sortu) kiçik hissələrdə bunu istifadə edir.
- ❌ Böyük, təsadüfi data — O(n²) yavaşdır.

## Əlaqəli
- [Bubble Sort](BubbleSort.md) · [Selection Sort](SelectionSort.md) — həm də O(n²).
- [Shell Sort](ShellSort.md) — Insertion Sort-un böyük addımlarla sürətləndirilmiş variantı.
- [Merge Sort](MergeSort.md) · [Quick Sort](QuickSort.md) — böyük data üçün O(n log n).
