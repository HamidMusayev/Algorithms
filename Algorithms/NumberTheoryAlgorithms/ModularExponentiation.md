# Modular Exponentiation (Modulyar Üstalma)

**Fikir:** `2^100`-ü birbaşa hesablamaq nəhəng ədəd verir. Amma yalnız `2^100 mod 1000` lazımdırsa, hər vurmada modul götürürsən — nəticə kiçik qalır. Hiyləgər hissə: `base`-i `exp` dəfə vurmaq əvəzinə **təkrar kvadrata yüksəltmə** istifadə et. `exp`-i ikilik göstər: `100 = 64+32+4`. Yalnız log₂(exp) vurma!

## Necə işləyir (iterativ)
1. `result = 1`, `base = base % m`.
2. `exp > 0` olduqca:
   - `exp` **tək**dirsə (son bit = 1): `result = (result × base) % m`.
   - Kvadrata yüksəlt: `base = (base × base) % m`.
   - Sürüşdür: `exp >>= 1`.
3. `result` qaytar.

## Nümunə
`2^10 mod 1000`, 10 = `1010₂`
| exp | tək? | result | base |
|-----|------|--------|------|
| 10 | yox | 1 | 4 |
| 5 | bəli | 4 | 16 |
| 2 | yox | 4 | 256 |
| 1 | bəli | 1024%1000=**24** | — |

`2^10 mod 1000 = 24` ✅

## Kod
```python
def mod_pow(base, exp, m):
    if m == 1:
        return 0
    result = 1
    base %= m
    while exp > 0:
        if exp & 1:                           # bit 1-dirsə
            result = (result * base) % m
        base = (base * base) % m              # kvadrata yüksəlt
        exp >>= 1
    return result

# Python-un daxili pow(base, exp, m) eyni işi C-də edir — istehsalda onu üstün tut.
print(mod_pow(2, 10, 1000))   # 24

def mod_inverse(a, p):
    """Fermat ilə modulyar tərs: a^(p-2) mod p (p sadə olmalıdır)."""
    return mod_pow(a, p - 2, p)
print(mod_inverse(3, 11))     # 4  (3 × 4 = 12 ≡ 1 mod 11)
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(log exp) |
| Yaddaş | O(1) iterativ |

## Nə vaxt
- ✅ `(a^b) % m`, `b` çox böyük olanda; modulyar arifmetika (yarışma proqramlaşdırması).
- ✅ Sadə modullu modulyar tərs (`pow(a, p-2, p)`); kriptoqrafiya (RSA).
- ✅ Polinomial heşləmə (Rabin-Karp-da).
- ❌ Sadə olmayan modullu tərs — Genişləndirilmiş GCD.
- ❌ Float üstalma — yalnız tam ədədlər.

## Əlaqəli
- [GCD](GCD.md) — Genişləndirilmiş GCD də modulyar tərs hesablayır (istənilən modul üçün).
- [Fermat's Theorem](FermatTheorem.md) — `a^(p-2) mod p` tərs düsturunu əsaslandırır.
- [Miller-Rabin](MillerRabin.md) — əsas əməliyyat kimi modulyar üstalma istifadə edir.
