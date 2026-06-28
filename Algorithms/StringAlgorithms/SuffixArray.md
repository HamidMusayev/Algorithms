# Suffix Array & LCP Array

**Fikir:** **Suffiks massivi** sətirin bütün suffikslərinin sıralanmış sırasıdır. `"banana"` üçün suffikslər sıralı: ["a", "ana", "anana", "banana", "na", "nana"] → suffiks massivi = [5, 3, 1, 0, 4, 2]. Sonra istənilən naxışı **binary axtarışla** O(m log n)-də tapırsan. **LCP (Ən uzun ortaq prefiks) massivi** ilə birlikdə çoxlu sətir sualını O(1) və ya O(log n)-də cavablandırır.

## Necə işləyir
**Suffiks massivi (sadə O(n² log n)):** bütün suffiksləri başlanğıc indeksləri kimi yarat, faktiki sətrə görə sırala.

**LCP massivi (Kasai alqoritmi — O(n)):**
1. Suffiks massivinin tərsini (`rank`) qur.
2. Orijinal sıra ilə hər suffiks üçün sıralı sıradakı əvvəlki ilə LCP hesabla.
3. Açar fikir: `i` ilə sələfinin LCP-si `h > 0`-dırsa, `i+1`-in LCP-si `≥ h-1` (hər addımda LCP ən çox 1 azalır).

## Nümunə
`"banana"` → suffiks massivi: `[5, 3, 1, 0, 4, 2]`, LCP: `[1, 3, 0, 0, 2]`

## Kod
```python
def suffix_array(s):
    """Sadə O(n² log n)."""
    return sorted(range(len(s)), key=lambda i: s[i:])

def build_lcp_array(s, sa):
    """Kasai alqoritmi — O(n)."""
    n = len(s)
    rank = [0] * n
    for i, start in enumerate(sa):
        rank[start] = i
    lcp = [0] * (n - 1)
    h = 0
    for i in range(n):
        if rank[i] > 0:
            j = sa[rank[i] - 1]
            while i + h < n and j + h < n and s[i + h] == s[j + h]:
                h += 1
            lcp[rank[i] - 1] = h
            if h > 0:
                h -= 1                      # LCP ən çox 1 azalır
    return lcp

s = "banana"
sa = suffix_array(s)
print(sa)                      # [5, 3, 1, 0, 4, 2]
print(build_lcp_array(s, sa))  # [1, 3, 0, 0, 2]
```

## Mürəkkəblik
| Əməliyyat | Vaxt |
|-----------|------|
| Suffiks massivi (sadə) | O(n² log n) |
| Suffiks massivi (SA-IS/DC3) | O(n) |
| LCP massivi (Kasai) | O(n) |
| Naxış axtarışı | O(m log n) |
| Yaddaş | O(n) |

## Nə vaxt
- ✅ Eyni sətir üzərində **çoxlu fərqli alt-sətir sorğusu** (O(n log n) hazırlıqdan sonra hər sorğu O(m log n)).
- ✅ Ən uzun təkrarlanan alt-sətir (LCP-də maksimum); fərqli alt-sətirlərin sayı (n(n+1)/2 − ΣLCP).
- ✅ Bioinformatika (uzun DNA ardıcıllıqları).
- ❌ Bir dəfəlik tək naxış axtarışı — KMP/Z daha sadədir.

## Əlaqəli
- [KMP](KMP.md) — tək naxış üçün yaxşı; mətn ön-emalı yoxdur.
- [Aho-Corasick](AhoCorasick.md) — mətn ön-emalı olmadan çoxnaxış.
- [Z-Algorithm](ZAlgorithm.md) — həm də O(n+m) naxış uyğunlaşması.
