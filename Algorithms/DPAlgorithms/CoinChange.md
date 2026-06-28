# Coin Change (Pul Üstü)

**Fikir:** Verilmiş nominallı sikkələrlə müəyyən məbləği yığırıq. Sual: **minimum neçə sikkə** lazımdır? Və ya **neçə yolla** yığmaq olar? Açar fikir: 1-dən `məbləğ-1`-ə qədər cavabı bilsən, hər sikkəni sınayaraq `məbləğ` üçün cavabı qurursan.

## Necə işləyir
**Variant 1 — minimum sikkə:**
`dp[0] = 0`, `dp[a] = min(dp[a - coin] + 1)` (hər `coin ≤ a` üçün).

**Variant 2 — yolların sayı:**
`dp[0] = 1`, `dp[a] += dp[a - coin]`.
> ⚠️ Dövr sırası vacibdir: **sikkə xaricdə, məbləğ daxildə** → sırasız kombinasiyalar. **Məbləğ xaricdə, sikkə daxildə** → sıralı permutasiyalar.

## Nümunə
Sikkələr = `[1, 2, 5]`, məbləğ = 6 (minimum)
| a | 0 | 1 | 2 | 3 | 4 | 5 | 6 |
|---|---|---|---|---|---|---|---|
| dp | 0 | 1 | 1 | 2 | 2 | 1 | **2** |

Cavab: 2 sikkə (5+1) ✅

## Kod
```python
def coin_change_min(coins, amount):
    """Minimum sikkə sayı; mümkün deyilsə -1."""
    dp = [float('inf')] * (amount + 1)
    dp[0] = 0
    for a in range(1, amount + 1):
        for coin in coins:
            if coin <= a:
                dp[a] = min(dp[a], dp[a - coin] + 1)
    return dp[amount] if dp[amount] != float('inf') else -1

def coin_change_ways(coins, amount):
    """Kombinasiyaların sayı (sırasız)."""
    dp = [0] * (amount + 1)
    dp[0] = 1
    for coin in coins:                  # sikkə xaricdə → kombinasiya
        for a in range(coin, amount + 1):
            dp[a] += dp[a - coin]
    return dp[amount]

print(coin_change_min([1, 2, 5], 11))   # 3  (5+5+1)
print(coin_change_ways([1, 2, 5], 5))    # 4
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n × məbləğ) (n = sikkə növü sayı) |
| Yaddaş | O(məbləğ) |

## Nə vaxt
- ✅ Verilmiş "ölçülü" elementlərlə (sikkə) hədəf məbləği yığmaq, **limitsiz sayda** istifadə.
- ✅ Hədəfə çatmağın yollarını saymaq (kombinasiya və ya permutasiya).
- ❌ Hər sikkə yalnız bir dəfə — 0/1 Knapsack istifadə et.
- ❌ Kanonik sikkə sistemləri (ABŋ sentləri) — acgöz (greedy) işləyir.

## Əlaqəli
- [Knapsack](Knapsack.md) — 0/1 = hər element bir dəfə; Coin Change = limitsiz.
- [Subset Sum](SubsetSum.md) — xüsusi hal: məbləği dəqiq yığmaq olarmı?
- [Fibonacci](Fibonacci.md) — pilləkən = [1,2] sikkələri ilə coin change.
