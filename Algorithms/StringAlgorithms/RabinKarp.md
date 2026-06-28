# Rabin-Karp Alqoritmi

**Fikir:** Sətirləri simvol-simvol müqayisə hər yoxlamada O(m)-dir. Rabin-Karp naxışın və hər mətn pəncərəsinin **heşini** hesablayır. Heşlər fərqlidirsə — atla. Eynidirsə — faktiki müqayisə ilə təsdiqlə (heş toqquşmasını rədd et). Sehr **dırnaqlı heşdir (rolling hash)**: pəncərə bir simvol sürüşəndə yeni heş O(1)-də hesablanır.

## Necə işləyir
1. Naxışın heşini polinomial dırnaqlı heşlə hesabla: `hash("abc") = ord('a')·base² + ord('b')·base + ord('c')  (mod prime)`.
2. İlk pəncərənin heşini hesabla.
3. Pəncərəni bir-bir sürüşdür:
   - `hash(pəncərə) == hash(naxış)` → faktiki müqayisə ilə təsdiqlə.
   - Heşi yenilə: solu çıxar, yeni sağı əlavə et.

## Nümunə
Mətn `"ABCABD"`, naxış `"ABD"`
- "ABC", "BCA", "CAB" → heş uyğun deyil
- "ABD" → heş uyğun → müqayisə → **indeks 3-də uyğunluq** ✅

## Kod
```python
def rabin_karp(text, pattern, base=256, mod=10**9 + 7):
    n, m = len(text), len(pattern)
    if m > n:
        return []
    high_power = pow(base, m - 1, mod)        # solu çıxarmaq üçün
    pat_hash = win_hash = 0
    for i in range(m):                         # ilkin heşlər
        pat_hash = (pat_hash * base + ord(pattern[i])) % mod
        win_hash = (win_hash * base + ord(text[i])) % mod
    matches = []
    for i in range(n - m + 1):
        if win_hash == pat_hash and text[i:i+m] == pattern:   # təsdiqlə
            matches.append(i)
        if i < n - m:                          # pəncərəni sürüşdür
            win_hash = (win_hash - ord(text[i]) * high_power) * base + ord(text[i+m])
            win_hash %= mod
    return matches

print(rabin_karp("GEEKS FOR GEEKS", "GEEK"))   # [0, 10]
print(rabin_karp("aaaa", "aa"))                 # [0, 1, 2]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Hazırlıq | O(m) |
| Axtarış (orta) | O(n + m) |
| Axtarış (ən pis) | O(n × m) (çox toqquşma) |
| Yaddaş | O(1) əlavə |

Yaxşı heş funksiyası ilə toqquşma çox nadirdir.

## Nə vaxt
- ✅ Eyni uzunluqlu **bir neçə naxışı** eyni anda axtarmaq (hamısını heş dəstinə yığ).
- ✅ Plagiat / təkrar məzmun aşkarlama; 2D naxış uyğunlaşması.
- ❌ Tək naxış və ən pis hal zəmanəti lazımdır — KMP (O(n+m) zəmanətli).

## Əlaqəli
- [KMP](KMP.md) — zəmanətli O(n+m); tək naxış üçün daha yaxşı.
- [Z-Algorithm](ZAlgorithm.md) — həm də O(n+m); KMP-dən sadə kod.
- [Boyer-Moore](BoyerMoore.md) — uzun naxışlarda praktikada sürətli.
- [Aho-Corasick](AhoCorasick.md) — çoxnaxışlı, amma fərqli uzunluqlu naxışlar üçün.
