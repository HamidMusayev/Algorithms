# Boyer-Moore Alqoritmi

**Fikir:** Əksər alqoritmlər soldan-sağa gəzir. Boyer-Moore naxışı mətn pəncərəsinə qarşı **sağdan-sola** yoxlayır. Uyğunsuzluqda iki heuristika nə qədər sıçramağı deyir:
1. **Pis simvol qaydası:** uyğunsuz mətn simvolu naxışda yoxdursa → onu keç.
2. **Yaxşı suffiks qaydası:** uyğun gələn suffiks naxışda başqa yerdə də varsa → o təkrarla düzləşdir.

Nəticə: praktikada böyük mətn parçalarını atlayır — orta halda **xəttialtı (sublinear)**, böyük əlifba üçün ən sürətli praktik axtarış.

## Necə işləyir
**Pis simvol heuristikası (sadələşdirilmiş):**
1. Cədvəl qur: hər `c` simvolu üçün `bad[c]` = naxışda son mövqeyi (yoxdursa -1).
2. `pattern[j] ≠ text[s+j]` olanda: `shift = j - bad.get(text[s+j], -1)`; pəncərəni `max(1, shift)` qədər irəli sür.

(Aşağıdakı kod yalnız pis simvol heuristikasını işlədir — sadədir, amma praktikada çox sürətli.)

## Nümunə
Mətn `"ABAAABCDABCDABDE"`, naxış `"ABCD"`
Pis simvol cədvəli: A→0, B→1, C→2, D→3 → uyğunluqlar: **4, 8**

## Kod
```python
def boyer_moore(text, pattern):
    n, m = len(text), len(pattern)
    if m == 0 or m > n:
        return []
    bad = {c: i for i, c in enumerate(pattern)}   # hər simvolun son mövqeyi
    matches, s = [], 0
    while s <= n - m:
        j = m - 1
        while j >= 0 and pattern[j] == text[s + j]:   # sağdan-sola
            j -= 1
        if j < 0:                                 # tam uyğunluq
            matches.append(s)
            s += (m - bad[text[s + m]] if s + m < n and text[s + m] in bad else 1)
        else:                                     # pis simvol sıçrayışı
            s += max(1, j - bad.get(text[s + j], -1))
    return matches

print(boyer_moore("ABAAABCDABCDABDE", "ABCD"))   # [4, 8]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Hazırlıq | O(m + σ) (σ = əlifba ölçüsü) |
| Axtarış (orta) | O(n / m) — xəttialtı! |
| Axtarış (ən pis) | O(n × m) (yalnız pis simvol) |
| Yaddaş | O(m + σ) |

## Nə vaxt
- ✅ Böyük əlifbalı (ingilis mətni, mənbə kod) böyük mətndə axtarış; uzun naxış (böyük sıçrayış).
- ✅ Praktik sürət nəzəri zəmanətdən vacibdir; grep tipli alətlər.
- ❌ Kiçik əlifba (DNA A/C/G/T, ikilik) — sıçrayışlar kiçikdir; KMP/Z daha yaxşı.
- ❌ Çoxnaxış — Aho-Corasick. Ciddi O(n+m) zəmanəti — KMP.

## Əlaqəli
- [KMP](KMP.md) — soldan-sağa; zəmanətli O(n+m); kiçik əlifba üçün yaxşı.
- [Z-Algorithm](ZAlgorithm.md) — həm də O(n+m); sadə kod.
- [Rabin-Karp](RabinKarp.md) · [Aho-Corasick](AhoCorasick.md) — çoxnaxış üçün.
