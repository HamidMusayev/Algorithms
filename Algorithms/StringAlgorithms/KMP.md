# KMP Alqoritmi (Knuth–Morris–Pratt)

**Fikir:** Mətndə "ABCABD" axtarırsan, "ABCAB" tapırsan, növbəti simvol səhvdir. Sadə oxucu əvvəldən başlayır. KMP başa düşür ki, uyğunlaşdırdığın sonundakı "AB" naxışın əvvəlindəki "AB" ilə eynidir — ona görə 0-a yox, yalnız 2-yə qayıdırsan.

Açar fikir: naxışın hər prefiksi üçün **həm prefiks, həm suffiks olan ən uzun hissəni (LPS)** əvvəlcədən hesabla. Naxışda `j` mövqeyində uyğunsuzluqda `j`-ni 0-a yox, `lps[j-1]`-ə atırsan.

## Necə işləyir
**Faza 1 — LPS cədvəlini qur:**
1. `lps[0] = 0`.
2. `length` (əvvəlki ən uzun prefiks-suffiks) və `i=1`.
3. `pattern[i] == pattern[length]`: `lps[i] = length + 1`, hər ikisini irəlilət.
4. Fərqlidirsə və `length > 0`: `length = lps[length - 1]` (i-ni irəlilətmə).
5. Fərqlidirsə və `length == 0`: `lps[i] = 0`, i-ni irəlilət.

**Faza 2 — Axtarış:** `text[i] == pattern[j]` olduqca hər ikisini irəlilət; `j == m` olanda uyğunluğu `i-j`-də qeyd et və `j = lps[j-1]`; uyğunsuzluqda `j>0` ⇒ `j = lps[j-1]`, yoxsa `i++`.

## Nümunə
Naxış: `"ABAB"` → LPS: `[0, 0, 1, 2]`
Mətn: `"ABABCABABAB"` → uyğunluqlar: **0, 5, 7**

## Kod
```python
def build_lps(pattern):
    m = len(pattern)
    lps = [0] * m
    length, i = 0, 1
    while i < m:
        if pattern[i] == pattern[length]:
            length += 1
            lps[i] = length
            i += 1
        elif length != 0:
            length = lps[length - 1]      # i-ni irəlilətmə
        else:
            lps[i] = 0
            i += 1
    return lps

def kmp_search(text, pattern):
    if not pattern:
        return []
    n, m = len(text), len(pattern)
    lps = build_lps(pattern)
    matches, i, j = [], 0, 0
    while i < n:
        if text[i] == pattern[j]:
            i += 1; j += 1
        if j == m:
            matches.append(i - j)
            j = lps[j - 1]                # suffiksi təkrar istifadə et
        elif i < n and text[i] != pattern[j]:
            j = lps[j - 1] if j != 0 else j
            if j == 0:
                i += 1
    return matches

print(kmp_search("ababcababab", "abab"))   # [0, 5, 7]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| LPS qurma | O(m) |
| Axtarış | O(n) |
| Ümumi | O(n + m) |
| Yaddaş | O(m) |

## Nə vaxt
- ✅ Mətndə sabit naxışın bütün təkrarlarını xətti vaxtda tapmaq.
- ✅ Bir sətirin digərinin fırlanması olub-olmadığı (`s + s`-də axtar); ən qısa period.
- ❌ Böyük əlifba, praktik sürət vacibdir — Boyer-Moore daha sürətli.
- ❌ Bir çox fərqli naxış eyni anda — Aho-Corasick.

## Əlaqəli
- [Z-Algorithm](ZAlgorithm.md) — həm də O(n+m); fərqli cədvəl (Z-massiv), kodlaması sadə.
- [Rabin-Karp](RabinKarp.md) — heş əsaslı alternativ.
- [Boyer-Moore](BoyerMoore.md) — böyük əlifbada praktikada sürətli; sağdan-sola.
- [Aho-Corasick](AhoCorasick.md) — KMP-nin uğursuzluq keçidlərini trie-yə genişləndirir.
