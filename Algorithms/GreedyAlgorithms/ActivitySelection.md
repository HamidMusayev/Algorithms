# Activity Selection (Fəaliyyət Seçimi)

**Fikir:** Bir iclas otağı və başlanğıc/bitmə vaxtı olan iclaslar var. Üst-üstə düşmədən maksimum sayda iclas keçirmək istəyirik. Acgöz (greedy) fikir: **həmişə ən tez bitən iclası seç** — tez bitirsən, gələcəyə daha çox yer qalır.

## Necə işləyir
1. Bütün fəaliyyətləri **bitmə vaxtına görə** artan sırala.
2. **İlk** fəaliyyəti seç (ən tez bitən).
3. Hər növbəti fəaliyyət üçün: başlanğıcı son seçilənin bitmə vaxtından ≥ olarsa → uyğun → seç; yoxsa atla.

> Niyə işləyir: ən tez bitən uyğun fəaliyyəti seçmək həmişə ən az qədər yer qoyur (mübadilə arqumenti).

## Nümunə
Fəaliyyətlər (bitməyə görə): A1(1,4), A2(3,5), A3(0,6), A4(5,7), A5(8,11)
- A1 ✅ → A2 (3<4) ❌ → A3 (0<4) ❌ → A4 (5≥4) ✅ → A5 (8≥7) ✅
- Seçildi: **A1, A4, A5** (3 fəaliyyət) ✅

## Kod
```python
def activity_selection(activities):
    """activities: (başlanğıc, bitmə) cütləri."""
    activities = sorted(activities, key=lambda x: x[1])   # bitməyə görə sırala
    selected = [activities[0]]
    last_finish = activities[0][1]
    for start, finish in activities[1:]:
        if start >= last_finish:           # uyğun: son seçimdən sonra başlayır
            selected.append((start, finish))
            last_finish = finish
    return selected

activities = [(1,4), (3,5), (0,6), (5,7), (3,9), (5,9), (6,10), (8,11)]
print(activity_selection(activities))   # [(1,4), (5,7), (8,11)]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n log n) (sıralama) |
| Yaddaş | O(1) əlavə |

## Nə vaxt
- ✅ Üst-üstə düşməyən maksimum sayda interval/tapşırıq planlaşdırmaq (iclas otağı).
- ✅ Məqsəd SAY-dır (çəkili dəyər yox) — greedy işləyir.
- ❌ Ümumi mənfəəti (sayı yox) maksimumlaşdırmaq — çəkili interval planlaşdırma (DP) lazımdır.

## Əlaqəli
- [Job Scheduling](JobScheduling.md) — həm də greedy; hər tapşırıq 1 vahid çəkir.
- [Fractional Knapsack](FractionalKnapsack.md) — həm də greedy; bitmə vaxtı yox, dəyər/çəki nisbəti.
- [Huffman Coding](HuffmanCoding.md) — həm də greedy; tezliklərin prioritet növbəsi.
