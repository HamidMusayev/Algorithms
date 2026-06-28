# Aho-Corasick Alqoritmi

**Fikir:** KMP mətndə BİR naxış axtarır. Bəs 1000 naxış eyni anda lazımdırsa? KMP-ni 1000 dəfə işlətmək O(1000 × n)-dir. Aho-Corasick mətn üzərində BİR keçidlə edir — O(n + ümumi uyğunluq).

İdeya: bütün naxışların **trie**-sini (prefiks ağacı) qur. Sonra **uğursuzluq keçidləri** əlavə et (KMP-nin uğursuzluq funksiyası kimi, amma trie-yə ümumiləşmiş) ki, trie node-da uyğunsuzluq olanda mətni təkrar oxumadan harada davam edəcəyini biləsən.

## Necə işləyir
**Qurma fazası:**
1. Bütün naxışları **trie**-yə daxil et.
2. **Uğursuzluq keçidlərini** hesabla (BFS ilə): hər node üçün cari yolun həm də trie-də prefiks olan ən uzun suffiksinə işarə edir.
3. **Çıxış keçidləri:** naxış uyğunluqlarını uğursuzluq keçidləri ilə yay.

**Axtarış fazası:**
4. Mətni simvol-simvol avtomat boyunca gəz.
5. Hər mövqedə cari node-un simvol tilovunu izlə (yoxsa uğursuzluq keçidlərini).
6. Cari mövqedə bitən bütün naxışları topla.

## Nümunə
Naxışlar: `["he", "she", "his", "hers"]`, mətn: `"ushers"`
Nəticə: `[(1,"she"), (2,"he"), (2,"hers")]` ✅

## Kod
```python
from collections import deque

class AhoCorasick:
    def __init__(self, patterns):
        self.goto = [{}]                     # goto[node][char] = növbəti node
        self.fail = [0]                      # uğursuzluq keçidi
        self.output = [[]]                   # bu node-da bitən naxışlar
        for pattern in patterns:
            self._insert(pattern)
        self._build_failure_links()

    def _insert(self, word):
        node = 0
        for char in word:
            if char not in self.goto[node]:
                self.goto.append({}); self.fail.append(0); self.output.append([])
                self.goto[node][char] = len(self.goto) - 1
            node = self.goto[node][char]
        self.output[node].append(word)

    def _build_failure_links(self):
        q = deque()
        for char, child in self.goto[0].items():
            self.fail[child] = 0
            q.append(child)
        while q:
            node = q.popleft()
            for char, child in self.goto[node].items():
                q.append(child)
                fail = self.fail[node]
                while fail and char not in self.goto[fail]:
                    fail = self.fail[fail]
                self.fail[child] = self.goto[fail].get(char, 0)
                if self.fail[child] == child:
                    self.fail[child] = 0
                self.output[child] += self.output[self.fail[child]]

    def search(self, text):
        node, results = 0, []
        for i, char in enumerate(text):
            while node and char not in self.goto[node]:
                node = self.fail[node]
            node = self.goto[node].get(char, 0)
            for pattern in self.output[node]:
                results.append((i - len(pattern) + 1, pattern))
        return results

ac = AhoCorasick(["he", "she", "his", "hers"])
print(ac.search("ushers"))   # [(1,'she'), (2,'he'), (2,'hers')]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Hazırlıq | O(Σ|naxışlar| × σ) |
| Axtarış | O(n + ümumi uyğunluq) |
| Yaddaş | O(Σ|naxışlar| × σ) |

## Nə vaxt
- ✅ Bir keçiddə **bir çox naxışı eyni anda** axtarmaq (10+ naxış, uzun mətn).
- ✅ Antivirus, müdaxilə aşkarlama, açar söz filtri; bir dəfə qur, çox mətndə işlət.
- ❌ Tək naxış — KMP/Z daha sadə.
- ❌ Təxmini/fuzzy uyğunlaşma — yalnız dəqiq uyğunluq tapır.

## Əlaqəli
- [KMP](KMP.md) — Aho-Corasick KMP-ni çox naxışa ümumiləşdirir.
- [Z-Algorithm](ZAlgorithm.md) — yalnız tək naxış; sadə.
- [Rabin-Karp](RabinKarp.md) — eyni uzunluqlu çoxnaxış; heş əsaslı.
