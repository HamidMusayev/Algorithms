# Sieve of Eratosthenes (Eratosfen Ələyi)

**Fikir:** 2-dən N-ə qədər bütün ədədləri yaz. 2-ni saxla (sadədir), onun bütün misillərini (4, 6, 8...) pozur. Növbəti pozulmamış ədədə keç (3 — sadədir), misillərini poz. Təkrarla. Pozulmamış ədədlər N-ə qədər bütün sadə ədədlərdir. Optimallaşdırma: pozmaya `2p`-dən yox, `p²`-dən başla (kiçik misillər artıq pozulub).

## Necə işləyir
1. `is_prime[0..n]` — hamısı `True`.
2. `is_prime[0] = is_prime[1] = False`.
3. Hər `i` (2-dən √n-ə qədər):
   - `is_prime[i]` True-dursa: `i²`-dən başlayaraq `i`-nin bütün misillərini `False` işarələ.
4. `is_prime[i] == True` olan bütün indeksləri topla.

## Nümunə
20-yə qədər: 2 → 4,6,8...; 3 → 9,15...; √20 ≈ 4.5 → dayan.
Sadələr: `[2, 3, 5, 7, 11, 13, 17, 19]`

## Kod
```python
def sieve(n):
    if n < 2:
        return []
    is_prime = [True] * (n + 1)
    is_prime[0] = is_prime[1] = False
    for i in range(2, int(n ** 0.5) + 1):     # yalnız √n-ə qədər
        if is_prime[i]:
            for j in range(i * i, n + 1, i):  # i²-dən başla
                is_prime[j] = False
    return [i for i in range(n + 1) if is_prime[i]]

print(sieve(30))   # [2, 3, 5, 7, 11, 13, 17, 19, 23, 29]
```

> **SPF variantı:** hər ədədin ən kiçik sadə vuruğunu saxlasan, istənilən ədədi O(log n)-də vuruqlara ayıra bilərsən.

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n log log n) |
| Yaddaş | O(n) |

Praktikada demək olar xəttidir; hər ədədi tək-tək yoxlamaqdan (O(n√n)) çox sürətli.

## Nə vaxt
- ✅ N-ə qədər **bütün sadə ədədləri** yaratmaq; N-ə qədər çoxlu ilkinlik sorğusu (bir dəfə hesabla, O(1) cavab).
- ✅ Hər ədədin ən kiçik sadə vuruğu (sürətli vuruqlara ayırma).
- ❌ Çox böyük tək ədəd üçün ilkinlik testi — Miller-Rabin.
- ❌ N > ~10⁸ (yaddaş) — seqmentli ələk.

## Əlaqəli
- [Miller-Rabin](MillerRabin.md) — böyük ədədlər üçün tək-ədəd ilkinlik testi.
- [GCD](GCD.md) · [Modular Exponentiation](ModularExponentiation.md) — ədədlər nəzəriyyəsində birgə işlənir.
