# 0/1 Knapsack (Bel Çantası)

**Fikir:** Çantan ən çox `W` kq tutur. Hər əşyanın çəkisi və dəyəri var; hər birini ya **götür**, ya **burax** (bölmək olmaz). Çəki həddini aşmadan ümumi dəyəri maksimum et. Hər əşya üçün cəmi iki seçim — bu "0/1"-in mənasıdır.

## Necə işləyir
**Rekurrens:** `dp[i][w]` = ilk i əşya, w tutum ilə maksimum dəyər.
- Əşya sığmırsa (`weight[i] > w`): `dp[i][w] = dp[i-1][w]`.
- Sığırsa: `dp[i][w] = max(dp[i-1][w], dp[i-1][w - weight[i]] + value[i])`.

1. `dp[0][w] = 0` (əşya yox = dəyər yox).
2. Hər əşya və hər tutum üçün: ya keç, ya götür — maksimumu seç.
3. Cavab `dp[n][W]`.

## Nümunə
weights=[1,3,4,5], values=[1,4,5,7], W=7
→ `dp[4][7] = 9` (əşya 2+3: 3+4=7 kq, 4+5=9 dəyər) ✅

## Kod
```python
def knapsack_01(weights, values, W):
    """2D DP cədvəli — aydın və başa düşülən."""
    n = len(weights)
    dp = [[0] * (W + 1) for _ in range(n + 1)]
    for i in range(1, n + 1):
        for w in range(W + 1):
            dp[i][w] = dp[i - 1][w]                 # əşya i-ni götürmə
            if weights[i - 1] <= w:                 # sığırsa götürməyi sına
                take = dp[i - 1][w - weights[i - 1]] + values[i - 1]
                dp[i][w] = max(dp[i][w], take)
    return dp[n][W]

def knapsack_optimized(weights, values, W):
    """Yaddaş-optimal O(W). Tutumu geriyə gəz — hər əşya bir dəfə."""
    dp = [0] * (W + 1)
    for i in range(len(weights)):
        for w in range(W, weights[i] - 1, -1):
            dp[w] = max(dp[w], dp[w - weights[i]] + values[i])
    return dp[W]

print(knapsack_01([1,3,4,5], [1,4,5,7], 7))   # 9
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n × W) |
| Yaddaş | O(n × W) → O(W)-yə endirilə bilər |

Psevdo-polinomdur (W-yə görə polinom, amma W böyük ola bilər).

## Nə vaxt
- ✅ Tutum məhdudiyyəti ilə dəyəri maksimumlaşdıran **alt-çoxluq seçimi**; hər əşya ən çox bir dəfə.
- ✅ Resurs bölgüsü, portfel seçimi.
- ❌ Əşyalar hissələrə bölünə bilir — Fractional Knapsack (greedy).
- ❌ Əşyalar limitsiz sayda — Unbounded Knapsack / Coin Change.

## Əlaqəli
- [Subset Sum](SubsetSum.md) — xüsusi hal: dəyər=çəki, maksimum yox, hədəf cəm.
- [Fractional Knapsack](../GreedyAlgorithms/FractionalKnapsack.md) — bölünən əşyalar → greedy.
- [Coin Change](CoinChange.md) — limitsiz knapsack variantı.
