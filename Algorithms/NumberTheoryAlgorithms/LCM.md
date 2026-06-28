# Ən Kiçik Ortaq Bölünən (LCM)

**Fikir:** Bir svetofor 3 dəqiqədə, digəri 4 dəqiqədə dövr edir. Eyni anda nə vaxt yaşıl yanırlar? LCM(3, 4) = 12 dəqiqədən bir. LCM iki ədədin **hər ikisinə bölünən ən kiçik müsbət tam ədəddir**. GCD ilə gözəl əlaqə: `lcm(a, b) = |a × b| / gcd(a, b)`.

## Necə işləyir
Düstur: `lcm(a, b) = |a × b| / gcd(a, b)`.
Siyahı üçün cüt-cüt tətbiq et: `lcm(a, b, c) = lcm(lcm(a, b), c)`.

> Niyə: hər sadə vuruq LCM-də **maksimum**, GCD-də **minimum** dərəcədə görünür. a×b hər ortaq vuruğu iki dəfə sayır, GCD-yə bölmək bunu aradan qaldırır.

## Nümunə
`lcm(12, 18)`: gcd = 6 → `216 / 6 = 36` ✅
`lcm([4, 6, 8])`: lcm(4,6)=12 → lcm(12,8)=24 ✅

## Kod
```python
from math import gcd
from functools import reduce

def lcm(a, b):
    if a == 0 or b == 0:
        return 0
    return abs(a * b) // gcd(a, b)        # // ilə float-dan qaç

def lcm_list(nums):
    return reduce(lcm, nums, 1)

# Qeyd: Python 3.9+ daxili math.lcm var.
print(lcm(12, 18))           # 36
print(lcm_list([4, 6, 8]))   # 24
print(lcm_list([3, 5, 7]))   # 105 (hamısı sadə → hasil)
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(log min(a, b)) (GCD üstünlük təşkil edir) |
| Yaddaş | O(1) |

## Nə vaxt
- ✅ Təkrarlanan hadisələrin **sinxronlaşması** (növbəti eyni anlı baş vermə).
- ✅ Kəsrləri toplamaq üçün ortaq məxrəc; bir çoxluğun ən kiçik ortaq bölünəni.
- ❌ Float ədədlər — yalnız tam ədədlər.
- ❌ Çox böyük ədədlər (a×b daşır) — əvvəlcə gcd hesabla.

## Əlaqəli
- [GCD](GCD.md) — LCM GCD ilə hesablanır: lcm = a×b / gcd(a,b).
- [Chinese Remainder Theorem](ChineseRemainderTheorem.md) — CRT qarşılıqlı sadə modullar tələb edir.
