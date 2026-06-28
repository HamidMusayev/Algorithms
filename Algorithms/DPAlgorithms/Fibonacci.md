# Fibonacci (Dinamik Proqramlaşdırma)

**Fikir:** `n`-ci pilləyə neçə yolla çatmaq olar? Yalnız `n-1` və `n-2`-ni bilmək kifayətdir. Sadə rekursiyada `fib(3)` dəfələrlə hesablanır → O(2ⁿ). DP hər nəticəni yadda saxlayır ki, hər alt-məsələ bir dəfə həll olunsun.

## Necə işləyir
1. **Baza:** `F(0)=0`, `F(1)=1`.
2. **Rekurrens:** `F(n) = F(n-1) + F(n-2)`, n ≥ 2.
3. **Yuxarıdan (memoizasiya):** adi rekursiya, amma hər dəyəri saxla; təkrar çağırışda keşdən qaytar.
4. **Aşağıdan (cədvəl):** dp massivini soldan-sağa doldur: `dp[i] = dp[i-1] + dp[i-2]`.
5. **Yaddaş optimizasiyası:** yalnız son iki dəyər lazımdır → iki dəyişən kifayətdir (O(1)).

## Nümunə
`F(7)` cədvəllə:
| i | 0 | 1 | 2 | 3 | 4 | 5 | 6 | 7 |
|---|---|---|---|---|---|---|---|---|
| dp | 0 | 1 | 1 | 2 | 3 | 5 | 8 | **13** |

## Kod
```python
def fib_memo(n, memo=None):           # yuxarıdan (memoizasiya)
    if memo is None:
        memo = {}
    if n < 2:
        return n
    if n in memo:
        return memo[n]
    memo[n] = fib_memo(n - 1, memo) + fib_memo(n - 2, memo)
    return memo[n]

def fib(n):                           # yaddaş-optimal (O(1))
    a, b = 0, 1                       # a = F(i-2), b = F(i-1)
    for _ in range(n):
        a, b = b, a + b
    return a

print([fib(i) for i in range(8)])     # [0, 1, 1, 2, 3, 5, 8, 13]
```

## Mürəkkəblik
| Yanaşma | Vaxt | Yaddaş |
|---------|------|--------|
| Sadə rekursiya | O(2ⁿ) | O(n) |
| Memoizasiya / Cədvəl | O(n) | O(n) |
| Optimal (2 dəyişən) | O(n) | **O(1)** |

## Nə vaxt
- ✅ Ardıcıllığın n-ci həddi, hər hədd əvvəlkilərdən asılı olanda.
- ✅ Sadə rekursiya eyni çağırışı təkrar edir (üst-üstə düşən alt-məsələlər).
- ✅ "Neçə yol/yol/kombinasiya" tipli sayma məsələləri.
- ❌ Çox kiçik n üçün bir dəfə — sadə dövr daha təmizdir.

## Əlaqəli
- [Coin Change](CoinChange.md) — eyni "neçə yol" DP nümunəsi; Fibonacci [1,2] sikkələri ilə xüsusi haldır.
- [Knapsack](Knapsack.md) — həm də bazadan qurulan 1D dp massivi.
- [LIS](LIS.md) — dp[i]-ni əvvəlki bütün dp[j]-dən qurur.
