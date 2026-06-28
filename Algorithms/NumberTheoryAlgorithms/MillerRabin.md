# Miller-Rabin İlkinlik Testi

**Fikir:** Fermat testi **Karmaykl ədədlərində** (hər şahidi aldadan mürəkkəb ədədlər) uğursuz olur. Miller-Rabin daha güclüdür: əlavə "1-in mod n kvadrat kökü" yoxlaması istifadə edir ki, Karmaykl ədədləri onu aldada bilmir. Açar fikir: sadə `p` üçün 1-in yeganə kvadrat kökləri ±1-dir. Kvadrata yüksəltmə zamanı `x² ≡ 1 (mod n)` amma `x ≢ ±1` tapsaq, `n` qəti mürəkkəbdir.

## Necə işləyir
`n - 1 = 2^r × d` yaz (d tək).
Şahid `a` üçün:
1. `x = a^d mod n` hesabla.
2. `x == 1` və ya `x == n-1` → çox güman sadə (növbəti şahidə keç).
3. `r-1` dəfə təkrarla: `x = x² mod n`.
   - `x == n-1` → çox güman sadə (daxili dövrü qır).
   - `x == 1` (əvvəlcə −1 görmədən) → mürəkkəb.
4. Daxili dövr `n-1` tapmadan bitsə → mürəkkəb.

> Determinik şahid dəsti `{2,3,5,...,37}` bütün `n < 3.3×10²⁴` üçün düzgündür.

## Nümunə
`n = 561` (Karmaykl): `560 = 2^4 × 35`, şahid a=2
`2^35 mod 561 = 263` → kvadratlar: 166 → 67 → 1 (amma 560 görünmədi) → **MÜRƏKKƏB** ✅

## Kod
```python
def miller_rabin(n, witnesses=(2,3,5,7,11,13,17,19,23,29,31,37)):
    """n < 3.3×10²⁴ üçün determinik ilkinlik testi."""
    if n < 2:
        return False
    if n == 2:
        return True
    if n % 2 == 0:
        return False
    for p in witnesses:
        if n == p:
            return True
        if n % p == 0:
            return False
    d, r = n - 1, 0                          # n-1 = 2^r * d
    while d % 2 == 0:
        d //= 2; r += 1
    for a in witnesses:
        if a >= n:
            continue
        x = pow(a, d, n)
        if x == 1 or x == n - 1:
            continue
        for _ in range(r - 1):
            x = (x * x) % n
            if x == n - 1:
                break
        else:
            return False                     # −1 heç vaxt tapılmadı → mürəkkəb
    return True

print(miller_rabin(561))           # False (Fermat True deyərdi!)
print(miller_rabin(1_000_000_007)) # True
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Hər şahid | O(log³ n) |
| k şahid | O(k log³ n) |
| Yaddaş | O(1) |

## Nə vaxt
- ✅ **Böyük ədədlərin** (> 10⁶) ilkinliyini səmərəli yoxlamaq; kriptoqrafiya (RSA açar yaratma).
- ✅ Yarışmada n 10¹⁸-ə qədər; 64-bit üçün determinik şahid dəsti ilə 100% dəqiqlik.
- ❌ N-ə qədər bütün sadələr — Eratosfen ələyi.
- ❌ Ədədlər < 10⁶ — ələk + axtarış daha sürətli.

## Əlaqəli
- [Fermat's Theorem](FermatTheorem.md) — zəif versiya; Karmaykl ədədlərində uğursuz.
- [Sieve of Eratosthenes](SieveOfEratosthenes.md) — N-ə qədər bütün sadələr.
- [Modular Exponentiation](ModularExponentiation.md) — əsas alt-prosedur: `pow(a, d, n)`.
