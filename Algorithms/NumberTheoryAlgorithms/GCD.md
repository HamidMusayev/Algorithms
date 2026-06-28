# Ən Böyük Ortaq Bölən (GCD)

**Fikir:** İki ədədin GCD-si, kiçik ədədin və böyüyün kiçiyə bölünməsindən qalan qalığın GCD-si ilə eynidir (Evklid). Beləliklə məsələni biri 0 olana qədər kiçildirik. Son sıfırdan fərqli qalıq cavabdır.

## Necə işləyir
Evklid eyniliyi: `gcd(a, b) = gcd(b, a mod b)`, `gcd(a, 0) = a`.
1. `b == 0`-dırsa, `a` qaytar.
2. Yoxsa `(a, b)`-ni `(b, a mod b)` ilə əvəz et.
3. `b` 0 olana qədər təkrarla.

**Genişləndirilmiş Evklid** əlavə olaraq `a·x + b·y = gcd(a, b)` (Bezu eyniliyi) üçün `x`, `y` tapır — modulyar tərslərin əsasıdır.

## Nümunə
`gcd(48, 18)`:
| a | b | a mod b |
|---|---|---------|
| 48 | 18 | 12 |
| 18 | 12 | 6 |
| 12 | 6 | 0 |

b=0 → **gcd(48, 18) = 6**

## Kod
```python
def gcd(a, b):
    while b:
        a, b = b, a % b       # (a,b) → (b, a mod b)
    return a

def extended_gcd(a, b):
    """a*x + b*y = g = gcd(a,b) üçün (g, x, y) qaytarır."""
    if b == 0:
        return a, 1, 0
    g, x1, y1 = extended_gcd(b, a % b)
    return g, y1, x1 - (a // b) * y1

print(gcd(48, 18))              # 6
print(extended_gcd(30, 18))    # (6, -1, 2) → 30·(-1) + 18·2 = 6
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(log min(a, b)) |
| Yaddaş | O(1) iterativ |

## Nə vaxt
- ✅ İki/çox ədədin ən böyük ortaq bölənini tapmaq; kəsri sadələşdirmək.
- ✅ Modulyar tərs (Genişləndirilmiş GCD); iki ədədin qarşılıqlı sadə olub-olmadığı (`gcd == 1`).
- ✅ LCM (`a * b // gcd(a, b)`); kriptoqrafiya (RSA, Diffie-Hellman).
- ❌ İlkinlik yoxlaması — Sieve / Miller-Rabin.
- ❌ Float ədədlər — GCD yalnız tam ədədlər üçündür.

## Əlaqəli
- [LCM](LCM.md) — `lcm = a·b // gcd(a,b)`.
- [Modular Exponentiation](ModularExponentiation.md) · [Fermat's Little Theorem](FermatTheorem.md) — modulyar tərs üçün.
- [Chinese Remainder Theorem](ChineseRemainderTheorem.md) — daxildə Genişləndirilmiş GCD istifadə edir.
