# Huffman Coding (Huffman Kodlaması)

**Fikir:** Tez-tez işlənən simvollara ("e", "t") qısa, nadir simvollara ("q", "z") uzun ikilik kodlar veririk. Acgöz seçim: hər addımda prioritet növbədə **ən az tezlikli iki node-u birləşdir**. Nadir simvollar ağacda ən dərində qalır, tezlər kökə yaxınlaşıb ən qısa kodu alır. Nəticə: qlobal optimal **prefiks-azad** kod.

## Necə işləyir
1. Hər unikal simvolun tezliyini say.
2. Hər simvol üçün yarpaq node yarat, min-topaya (tezliyə görə) qoy.
3. **Acgöz:** ən kiçik tezlikli iki node-u (`left`, `right`) çıxar.
4. Tezliyi `left.freq + right.freq` olan yeni daxili node yarat (uşaqları left və right).
5. Yeni node-u topaya qaytar.
6. Bir node qalana qədər təkrarla — bu, Huffman ağacının köküdür.
7. Kökdən gəz: **sol** → "0", **sağ** → "1". Hər yarpağın bit sətiri o simvolun kodudur.

## Nümunə
`"aabbbccdd"` → tezliklər: a:2, c:2, d:2, b:3
- Birləşdir: a+c=4, d+b=5, sonra 4+5=9 (kök)
- Kodlar: a=00, c=01, d=10, b=11

## Kod
```python
import heapq
from collections import Counter

class Node:
    def __init__(self, freq, char=None, left=None, right=None):
        self.freq, self.char, self.left, self.right = freq, char, left, right
    def __lt__(self, other):
        return self.freq < other.freq

def huffman_codes(text):
    if not text:
        return {}
    freq = Counter(text)
    if len(freq) == 1:                          # tək simvol halı
        return {next(iter(freq)): "0"}
    heap = [Node(f, c) for c, f in freq.items()]
    heapq.heapify(heap)
    while len(heap) > 1:
        left = heapq.heappop(heap)              # acgöz: ən kiçik tezlik
        right = heapq.heappop(heap)
        heapq.heappush(heap, Node(left.freq + right.freq, left=left, right=right))
    root = heap[0]
    codes = {}
    def walk(node, code):
        if node.char is not None:               # yarpaq → kodu yaz
            codes[node.char] = code
            return
        walk(node.left, code + "0")
        walk(node.right, code + "1")
    walk(root, "")
    return codes

print(sorted(huffman_codes("abracadabra").items()))
# məs. [('a','0'), ('b','111'), ('c','1101'), ('d','1100'), ('r','10')]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n log n) |
| Yaddaş | O(n) |

## Nə vaxt
- ✅ Simvol tezlikləri **qeyri-bərabər** olan datanı sıxmaq (zip, gzip, JPEG entropiya kodlaması).
- ✅ İkilik ağacda minimum ümumi çəkili yol uzunluğu; prefiks-azad kodlar.
- ❌ Bütün simvollar bərabər tezlikli — sabit uzunluqlu kod qədər fayda yoxdur.
- ❌ Sıxılmış dataya təsadüfi giriş lazımdır — Huffman axın kodudur.

## Əlaqəli
- [Activity Selection](ActivitySelection.md) · [Fractional Knapsack](FractionalKnapsack.md) — həm də greedy, prioritetə görə sıralayır.
- [Job Scheduling](JobScheduling.md) — həm də greedy planlaşdırma, oxşar topa yanaşması.
