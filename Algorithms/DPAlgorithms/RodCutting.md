# Rod Cutting (Çubuğun Kəsilməsi)

**Fikir:** `n` uzunluqlu polad çubuq və hər kəsim uzunluğu üçün qiymət siyahısı var. Çubuğu elə kəsmək istəyirik ki, ümumi gəlir maksimum olsun. Hər uzunluq limitsiz sayda istifadə oluna bilər — bu, mahiyyətcə **unbounded knapsack**-dır. `dp[i]` = i uzunluqlu çubuqdan maksimum gəlir.

## Necə işləyir
**Rekurrens:** `dp[0] = 0`, `dp[length] = max(price[i] + dp[length - i])`, i ∈ [1..length].

Hər çubuq uzunluğu üçün (1..n): hər mümkün ilk kəsimi `i` sına; gəlir = `price[i]` + qalanın optimalı `dp[length - i]`; maksimumu götür.

## Nümunə
Qiymətlər: `[1, 5, 8, 9, 10, 17, 17, 20]` (price[i] = i+1 uzunluğun qiyməti), n=8
→ `dp[8] = 22` (uzunluq 2 + uzunluq 6: 5 + 17 = 22) ✅

## Kod
```python
def rod_cutting(prices, n):
    """prices[i] = (i+1) uzunluğun qiyməti; n = çubuq uzunluğu."""
    dp = [0] * (n + 1)
    for length in range(1, n + 1):
        best = 0
        for cut in range(1, length + 1):    # hər mümkün ilk kəsim
            best = max(best, prices[cut - 1] + dp[length - cut])
        dp[length] = best
    return dp[n]

prices = [1, 5, 8, 9, 10, 17, 17, 20]
print(rod_cutting(prices, 8))   # 22
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n²) |
| Yaddaş | O(n) |

## Nə vaxt
- ✅ Bir resursu hissələrə bölərək ümumi dəyəri maksimumlaşdırmaq; hissə ölçüləri **təkrar** istifadə oluna bilir.
- ✅ "N-ə cəmlənən ölçü kombinasiyası seç (təkrarla), dəyəri maksimum et" quruluşu.
- ❌ Hər ölçü yalnız bir dəfə — 0/1 Knapsack.

## Əlaqəli
- [Knapsack](Knapsack.md) — Rod Cutting = unbounded knapsack.
- [Coin Change](CoinChange.md) — eyni nümunə: cəmə çatmaq üçün element seç.
- [Matrix Chain Multiplication](MatrixChainMultiplication.md) — həm də interval DP ("necə optimal bölmək").
