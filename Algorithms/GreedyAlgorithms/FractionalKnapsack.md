# Fractional Knapsack (Hissəli Bel Çantası)

**Fikir:** 50 kq-lıq çanta və müxtəlif qızıl külçələri. 0/1 Knapsack-dan fərqli olaraq külçəni **kəsmək** olar — lazım gəlsə 1/3-ini götür. Ona görə həmişə ən dəyərli materialı (ən yüksək dəyər/kq nisbəti) əvvəl götürürük, son əşyanı tam sığacaq qədər kəsirik.

## Necə işləyir
1. Hər əşya üçün **dəyər/çəki nisbəti** hesabla.
2. Əşyaları nisbətə görə azalan sırala.
3. Hər əşya üçün (nisbət sırası ilə):
   - Tam sığırsa: hamısını götür, tutumu azalt.
   - Sığmırsa: `qalan_tutum / çəki` hissəsini götür və dayan.

## Nümunə
weights=[10, 20, 30], values=[60, 100, 120], tutum=50
| Əşya | Nisbət (v/w) |
|------|--------------|
| 1 | 6.0 |
| 2 | 5.0 |
| 3 | 4.0 |

- Əşya 1: tam 10 kq → +60
- Əşya 2: tam 20 kq → +100
- Əşya 3: 20/30 hissəsi → +80
- Ümumi: **240.0** ✅

## Kod
```python
def fractional_knapsack(weights, values, capacity):
    n = len(weights)
    items = sorted(range(n), key=lambda i: values[i] / weights[i], reverse=True)
    total, remaining = 0.0, capacity
    for i in items:
        if remaining <= 0:
            break
        fraction = min(1.0, remaining / weights[i])   # mümkün qədər götür
        total += fraction * values[i]
        remaining -= fraction * weights[i]
    return total

print(fractional_knapsack([10, 20, 30], [60, 100, 120], 50))   # 240.0
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n log n) (sıralama) |
| Yaddaş | O(1) əlavə |

## Nə vaxt
- ✅ Əşyalar **bölünə bilir** (maye, sıyıq material, vaxt bölgüsü).
- ✅ Çəki/tutum məhdudiyyəti ilə dəyəri maksimum etmək.
- ❌ Əşyalar bölünmür (tam götür və ya heç) — 0/1 Knapsack (DP).

## Əlaqəli
- [Knapsack 0/1](../DPAlgorithms/Knapsack.md) — bölünməyən əşyalar; greedy işləmir, DP lazımdır.
- [Activity Selection](ActivitySelection.md) — həm də greedy.
- [Huffman Coding](HuffmanCoding.md) — həm də greedy.
