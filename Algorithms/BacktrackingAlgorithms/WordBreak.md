# Word Break (Sözə Bölmə)

**Fikir:** `"leetcode"` sətirini və `["leet", "code"]` lüğətini verib soruşuruq: sətiri düzgün lüğət sözlərinə bölmək olarmı? Backtracking hər mümkün bölünmə nöqtəsini sınayır, prefiks düzgün sözdürsə qalan hissədə rekursiya edir. Açar optimizasiya: dalana aparan başlanğıc mövqeləri **memoizasiya** et.

## Necə işləyir
**Mümkünlük (memoizasiya ilə):**
1. `s[start:]`-in hər mümkün bölünməsini sına — `s[start:end]` lüğətdədir?
2. Düzgün sözdürsə, `s[end:]`-də rekursiya et.
3. Uğursuzluğa aparan başlanğıc mövqeləri keşlə.
4. `start == len(s)` olanda True qaytar.

**Bütün bölünmələri sadalamaq:** eyni yanaşma, amma birinci yox, bütün düzgün yolları topla.

## Nümunə
s = `"catsanddog"`, words = `["cat","cats","and","sand","dog"]`
- "cat sand dog" ✅
- "cats and dog" ✅

## Kod
```python
def word_break(s, words):
    """s lüğət sözlərinə bölünə bilərmi?"""
    word_set = set(words)
    memo = {}
    def can_break(start):
        if start == len(s):
            return True
        if start in memo:
            return memo[start]
        for end in range(start + 1, len(s) + 1):
            if s[start:end] in word_set and can_break(end):
                memo[start] = True
                return True
        memo[start] = False                  # bu mövqedən dalan
        return False
    return can_break(0)

def word_break_all(s, words):
    """s-i bölməyin BÜTÜN yollarını tapır."""
    word_set = set(words)
    results = []
    def backtrack(start, path):
        if start == len(s):
            results.append(" ".join(path))
            return
        for end in range(start + 1, len(s) + 1):
            word = s[start:end]
            if word in word_set:
                path.append(word)
                backtrack(end, path)
                path.pop()                   # geri al
    backtrack(0, [])
    return results

print(word_break("leetcode", ["leet", "code"]))             # True
print(word_break_all("catsanddog", ["cat","cats","and","sand","dog"]))
# ['cat sand dog', 'cats and dog']
```

## Mürəkkəblik
| Yanaşma | Vaxt | Yaddaş |
|---------|------|--------|
| Sırf backtracking | O(2ⁿ) | O(n) |
| Memoizasiya ilə | O(n² × L) | O(n) |

L = maksimum söz uzunluğu.

## Nə vaxt
- ✅ Sətiri lüğət sözlərinə bölmək (söz seqmentasiyası, tokenizasiya — NLP, kompilyator lexer-i).
- ✅ Bölünə bilərmi (memoizasiya) və ya bütün bölünmələr (sırf backtracking).
- ❌ Çox uzun sətirlər — trie əsaslı optimizasiya lazımdır.
- ❌ Təxmini uyğunlaşdırma (səhvlərə icazə) — edit distance / fuzzy matching.

## Əlaqəli
- [Subset Sum Backtracking](SubsetSumBacktracking.md) — eyni sına→rekursiya→geri al nümunəsi.
- [N-Queens](NQueens.md) — eyni backtracking quruluşu.
- [KMP](../StringAlgorithms/KMP.md) — səmərəli alt-sətir uyğunlaşması.
