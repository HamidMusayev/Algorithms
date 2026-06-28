# Subset Sum (Alt-Çoxluq Cəmi)

**Fikir:** Verilmiş ədədlərdən bəzilərini seçərək (və ya heç birini) dəqiq hədəf cəmə çatmaq mümkündürmü? `dp[i][s]` = ilk i ədədlə s cəminə çatmaq olarmı? Cavab ya s-ə i-siz çatmaqdan, ya da `s - nums[i]`-ə çatıb i-ni daxil etməkdən asılıdır.

## Necə işləyir
**Rekurrens:**
- `nums[i-1] > s` (sığmır): `dp[i][s] = dp[i-1][s]`.
- Yoxsa: `dp[i][s] = dp[i-1][s] OR dp[i-1][s - nums[i-1]]`.

**Baza:** `dp[i][0] = True` (boş çoxluq həmişə 0 verir), `dp[0][s>0] = False`.

## Nümunə
nums = `[3, 4, 2]`, hədəf = 6
| | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|---|
| [3,4,2] | ✅ | ❌ | ✅ | ✅ | ✅ | ✅ | **✅** |

`dp = True` → {4, 2} = 6 ✅

## Kod
```python
def subset_sum(nums, target):
    n = len(nums)
    dp = [[False] * (target + 1) for _ in range(n + 1)]
    for i in range(n + 1):
        dp[i][0] = True                          # boş çoxluq → 0
    for i in range(1, n + 1):
        for s in range(1, target + 1):
            dp[i][s] = dp[i - 1][s]              # nums[i-1]-i daxil etmə
            if nums[i - 1] <= s:                 # daxil etməyi sına
                dp[i][s] = dp[i][s] or dp[i - 1][s - nums[i - 1]]
    return dp[n][target]

def subset_sum_optimized(nums, target):
    """Yaddaş-optimal O(target). Geriyə gəz."""
    dp = [False] * (target + 1)
    dp[0] = True
    for num in nums:
        for s in range(target, num - 1, -1):
            dp[s] = dp[s] or dp[s - num]
    return dp[target]

print(subset_sum([3, 34, 4, 12, 5, 2], 9))    # True (4+5)
print(subset_sum([3, 34, 4, 12, 5, 2], 30))   # False
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n × hədəf) |
| Yaddaş | O(n × hədəf) → O(hədəf)-ə endirilə bilər |

Psevdo-polinom; hədəf kiçik olanda yaxşı işləyir.

## Nə vaxt
- ✅ Hər hansı alt-çoxluğun hədəfə çatıb-çatmadığını yoxlamaq.
- ✅ Bölmə məsələsi (massivi iki bərabər yarıya bölmək: hədəf = cəm/2).
- ❌ Elementlər limitsiz sayda istifadə olunur — Coin Change (unbounded).
- ❌ Hədəf çox böyük — O(n × hədəf) yavaşdır.

## Əlaqəli
- [Knapsack](Knapsack.md) — Subset Sum = dəyər=çəki olan Knapsack.
- [Coin Change](CoinChange.md) — limitsiz versiya.
- [Subset Sum Backtracking](../BacktrackingAlgorithms/SubsetSumBacktracking.md) — bütün uyğun alt-çoxluqları sadalayır.
