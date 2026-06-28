# Z-Algorithm

**Fikir:** `s` sətirinin Z-massivi hər `i` mövqeyi üçün deyir: `i`-dən başlayan neçə simvol `s`-in əvvəli ilə uyğun gəlir? Yəni `Z[i]` = `s[i]`-dən başlayıb həm də `s`-in prefiksi olan ən uzun alt-sətrin uzunluğu. Naxış axtarışı üçün: `naxış + '$' + mətn` birləşdir; `Z[i] == len(naxış)` olan yerlər uyğunluqdur.

## Necə işləyir
**Z-massivini qurmaq:**
1. `s[l..r]`-in `s[0..r-l]` ilə uyğun gəldiyi `[l, r]` pəncərəsi saxla.
2. Hər `i` üçün:
   - `i < r`-dirsə: əvvəl hesablanmış dəyəri istifadə et: `min(r-i, Z[i-l])`.
   - Sadəcə uzat: uyğunsuzluğa qədər `s[i + Z[i]]` ilə `s[Z[i]]` müqayisə et.
   - Pəncərə r-dən kənara çıxıbsa `[l, r]`-i yenilə.

## Nümunə
`"aabxaa"` → Z-massiv: `[0, 1, 0, 0, 2, 1]`
Axtarış: naxış="aa", mətn="aabxaa" → uyğunluqlar: 0 və 5

## Kod
```python
def z_function(s):
    n = len(s)
    z = [0] * n
    l, r = 0, 0                              # cari uyğun pəncərə
    for i in range(1, n):
        if i < r:
            z[i] = min(r - i, z[i - l])      # əvvəlki dəyəri istifadə et
        while i + z[i] < n and s[z[i]] == s[i + z[i]]:
            z[i] += 1                         # uzat
        if i + z[i] > r:
            l, r = i, i + z[i]
    return z

def z_search(text, pattern):
    if not pattern or not text:
        return []
    combined = pattern + '$' + text
    z = z_function(combined)
    m = len(pattern)
    return [i - m - 1 for i in range(m + 1, len(combined)) if z[i] == m]

print(z_function("aabxaa"))            # [0, 1, 0, 0, 2, 1]
print(z_search("aaaa", "aa"))         # [0, 1, 2]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Z-massiv qurma | O(n) |
| Axtarış | O(n + m) |
| Yaddaş | O(n + m) (birləşmiş sətir) |

## Nə vaxt
- ✅ Naxış uyğunlaşması üçün KMP-yə sadə alternativ.
- ✅ Sətirin ən qısa periodu (`Z[p] = n - p` olan ən kiçik p); fırlanma yoxlaması (`s + s`-də axtar).
- ❌ Bir çox fərqli naxış — Aho-Corasick.
- ❌ Təxmini/fuzzy uyğunlaşma — edit distance.

## Əlaqəli
- [KMP](KMP.md) — həm də O(n+m); Z əvəzinə LPS cədvəli.
- [Rabin-Karp](RabinKarp.md) — heş əsaslı; eyni uzunluqlu çoxnaxış üçün.
- [Boyer-Moore](BoyerMoore.md) — sağdan-sola; böyük əlifbada sürətli.
- [Aho-Corasick](AhoCorasick.md) — çoxnaxışlı uyğunlaşma.
