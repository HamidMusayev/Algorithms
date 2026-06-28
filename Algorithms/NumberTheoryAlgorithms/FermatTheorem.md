# Fermat-ın Kiçik Teoremi

**Fikir:** `p` sadədirsə və `a` `p`-yə bölünməyən istənilən tam ədəddirsə:
`a^(p-1) ≡ 1 (mod p)`

Bu o deməkdir ki, bütün tam ədədlər üçün `a^p ≡ a (mod p)`. Sadə ədədlərin əsas xassəsidir. **Nə üçün vacibdir:** modul sadə olanda **modulyar tərsi** sürətli hesablamağa və sürətli (qeyri-mükəmməl) **ilkinlik testinə** imkan verir.

## Necə işləyir
**Tətbiq 1 — Modulyar tərs:**
`a × a^(p-2) = a^(p-1) ≡ 1 (mod p)` → `a^(-1) ≡ a^(p-2) (mod p)`.
`pow(a, p-2, p)` istifadə et — O(log p).

**Tətbiq 2 — Fermat ilkinlik testi:** namizəd `n` üçün təsadüfi şahidlər `a` seç:
- `a^(n-1) mod n ≠ 1` → `n` qəti **mürəkkəbdir**.
- `a^(n-1) mod n == 1` → `n` **çox güman ki** sadədir (amma Karmaykl ədədi ola bilər!).

## Nümunə
3-ün mod 11 tərsi (p=11 sadə):
`x = 3^(11-2) mod 11 = 3^9 mod 11 = 4`
Yoxla: `3 × 4 = 12 ≡ 1 (mod 11)` ✅

## Kod
```python
def mod_inverse_fermat(a, p):
    """a-nın mod p tərsi. ŞƏRT: p SADƏ olmalı, gcd(a, p)=1."""
    return pow(a, p - 2, p)

def fermat_test(n, k=10):
    """Ehtimallı ilkinlik testi. XƏBƏRDARLIQ: Karmaykl ədədlərində səhv (məs. 561)."""
    import random
    if n < 2:
        return False
    if n in (2, 3, 5, 7):
        return True
    if n % 2 == 0:
        return False
    for _ in range(k):
        a = random.randrange(2, n - 1)
        if pow(a, n - 1, n) != 1:
            return False                      # Fermat şərti pozuldu → mürəkkəb
    return True

print(mod_inverse_fermat(3, 11))   # 4
print(fermat_test(17))             # True
print(fermat_test(561))            # True ← SƏHV! 561 Karmaykl (mürəkkəb)
# Məhz buna görə praktikada Miller-Rabin üstün tutulur.
```

## Mürəkkəblik
| Əməliyyat | Vaxt |
|-----------|------|
| Modulyar tərs | O(log p) |
| İlkinlik testi (k tur) | O(k log² p) |
| Yaddaş | O(1) |

## Nə vaxt
- ✅ Modul **sadə** olanda modulyar tərs; `C(n,k) mod p` (kombinatorika).
- ✅ Kriptoqrafiya (RSA, Diffie-Hellman).
- ❌ Modul sadə deyil — tərs üçün Genişləndirilmiş GCD.
- ❌ Etibarlı ilkinlik testi — Miller-Rabin (Karmaykl ədədlərini düzgün tutur).

## Əlaqəli
- [GCD](GCD.md) — Genişləndirilmiş GCD istənilən modul üçün işləyir.
- [Modular Exponentiation](ModularExponentiation.md) — Fermat tərsi `pow(a, p-2, p)` ilə hesablanır.
- [Miller-Rabin](MillerRabin.md) — daha güclü ilkinlik testi; Karmaykl səhvi yoxdur.
