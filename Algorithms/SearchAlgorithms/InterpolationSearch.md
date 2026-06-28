# Interpolation Search

**Fikir:** Lüğətdə "Ant" sözünü ortadan yox, əvvələ yaxın açırsan. Bu axtarış da target-in **dəyərinə görə** ehtimal olunan mövqeyi təxmin edir — ortaya yox, proporsiyaya görə sıçrayır.

## Necə işləyir
1. İnterpolyasiya düsturu ilə mövqeyi təxmin et:
   `pos = low + (target - arr[low]) × (high - low) / (arr[high] - arr[low])`
2. `arr[pos] == target` → tapıldı.
3. `arr[pos] < target` → sağda axtar: `low = pos + 1`.
4. `arr[pos] > target` → solda axtar: `high = pos - 1`.
5. `low > high` və ya target diapazondan kənardırsa → `-1`.

## Nümunə
`[10, 20, 30, 40, 50, 60, 70, 80, 90]`, axtarılan = `70`
- pos = 0 + (70−10)×8/(90−10) = **6** → arr[6]=70 → **bir müqayisədə tapıldı** ✅
  (Binary Search ~3 müqayisə edərdi.)

## Kod
```python
def interpolation_search(arr, target):
    low, high = 0, len(arr) - 1
    while low <= high and arr[low] <= target <= arr[high]:
        if arr[low] == arr[high]:              # sıfıra bölmənin qarşısını al
            return low if arr[low] == target else -1
        pos = low + ((target - arr[low]) * (high - low) // (arr[high] - arr[low]))
        if arr[pos] == target:
            return pos
        elif arr[pos] < target:
            low = pos + 1
        else:
            high = pos - 1
    return -1

print(interpolation_search([10, 20, 30, 40, 50, 60, 70, 80, 90], 70))   # 6
```

## Mürəkkəblik
| Hal | Vaxt | Yaddaş |
|-----|------|--------|
| Orta (bərabər paylanmış) | O(log log n) | O(1) |
| Ən pis (qruplaşmış data) | O(n) | O(1) |

> O(log log n) yalnız **bərabər paylanmış** data üçündür. Binary Search isə həmişə O(log n)-dir.

## Nə vaxt
- ✅ Sıralı VƏ bərabər paylanmış dəyərlər (məs. ID-lər 1000–9999, yaşlar 0–100).
- ❌ Data qruplaşıb/əyilib — O(n)-ə deqradasiya edir, Binary Search istifadə et.
- ❌ Təkrar dəyərlər var və ya massiv kiçikdir (n < 100).

## Əlaqəli
- [Binary Search](BinarySearch.md) — paylanmadan asılı olmayaraq həmişə O(log n), daha etibarlı.
- [Jump Search](JumpSearch.md) — həm də sıralı massiv; O(√n), paylanma fərziyyəsi yoxdur.
- [Exponential Search](ExponentialSearch.md) — sonsuz massivlər üçün; içində Binary Search.
