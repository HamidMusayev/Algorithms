# Chinese Remainder Theorem (Çin Qalıq Teoremi)

**Fikir:** Qədim Çin sərkərdəsi əsgərlərini sayır: 3-lük cərgələrdə 2 artıq, 5-lik cərgələrdə 3 artıq, 7-lik cərgələrdə 2 artıq. CRT dəqiq sayı tapır. Sistemi həll edir:
```
x ≡ 2 (mod 3),  x ≡ 3 (mod 5),  x ≡ 2 (mod 7)
```
→ 3×5×7=105 moduluna görə yeganə həll: **x = 23**. **Modullar cüt-cüt qarşılıqlı sadə olmalıdır.**

## Necə işləyir
`N = n₁ × n₂ × ... × nₖ`. Hər `x ≡ aᵢ (mod nᵢ)` üçün:
1. `Mᵢ = N / nᵢ` (qalan bütün modulların hasili).
2. `yᵢ = Mᵢ⁻¹ mod nᵢ` (Mᵢ-nin mod nᵢ tərsi, Genişləndirilmiş GCD ilə).
3. Həll: `x = (Σ aᵢ × Mᵢ × yᵢ) mod N`.

> Hər `aᵢ × Mᵢ × yᵢ` həddi mod nᵢ-də `aᵢ` verir (çünki Mᵢ × yᵢ ≡ 1), mod nⱼ-də isə 0 (çünki nⱼ Mᵢ-ni bölür).

## Nümunə
`x ≡ 2(3), 3(5), 2(7)` → N=105
| aᵢ | nᵢ | Mᵢ | yᵢ | aᵢMᵢyᵢ |
|----|----|----|----|--------|
| 2 | 3 | 35 | 2 | 140 |
| 3 | 5 | 21 | 1 | 63 |
| 2 | 7 | 15 | 1 | 30 |

Cəm = 233; 233 mod 105 = **23** ✅

## Kod
```python
from math import gcd

def extended_gcd(a, b):
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    return g, y1, x1 - (a // b) * y1

def mod_inverse(a, m):
    g, x, _ = extended_gcd(a % m, m)
    if g != 1:
        raise ValueError("Tərs mövcud deyil (qarşılıqlı sadə deyil)")
    return x % m

def crt(remainders, moduli):
    """x ≡ remainders[i] (mod moduli[i]); modullar cüt-cüt sadə olmalıdır."""
    N = 1
    for n in moduli:
        N *= n
    x = 0
    for a, n in zip(remainders, moduli):
        M = N // n
        y = mod_inverse(M, n)
        x = (x + a * M * y) % N
    return x

print(crt([2, 3, 2], [3, 5, 7]))   # 23
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(k log N) (k konqruens, hər biri Genişləndirilmiş GCD) |
| Yaddaş | O(1) |

## Nə vaxt
- ✅ Eyni anda **çoxlu modulyar məhdudiyyəti** ödəyən ədəd tapmaq (modullar qarşılıqlı sadə).
- ✅ RSA deşifrəsinin optimallaşdırılması (CRT RSA-nı ~4 dəfə sürətləndirir).
- ❌ Modullar cüt-cüt sadə deyil — ümumiləşmiş CRT lazımdır.
- ❌ Yalnız bir məhdudiyyət — sadə modulyar arifmetika kifayətdir.

## Əlaqəli
- [GCD](GCD.md) — Genişləndirilmiş GCD lazım olan modulyar tərsi hesablayır.
- [Modular Exponentiation](ModularExponentiation.md) — RSA daxilində; CRT deşifrəni sürətləndirir.
- [Fermat's Theorem](FermatTheorem.md) — modul sadə olanda tərs üçün alternativ.
