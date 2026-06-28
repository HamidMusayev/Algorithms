# Floyd's Cycle Detection (Tısbağa və Dovşan)

**Fikir:** Dairəvi trekdə iki qaçışçı; biri ikiqat sürətli. Trekdə dövr varsa, sürətli olan yavaşı dövrələyib mütləq ona çatacaq. Floyd alqoritmi bunu bağlı siyahıya tətbiq edir: iki göstərici (yavaş və sürətli); görüşürlərsə, dövr var. Bonus: görüş nöqtəsindən sonra **dövrün haradan başladığını** da tapmaq olar.

## Necə işləyir
**Faza 1 — dövr aşkarla:**
1. `slow = fast = head`.
2. Hər iterasiyada `slow` 1, `fast` 2 addım.
3. `fast` None-a çatsa → **dövr yoxdur**.
4. `slow == fast` → **dövr var** (dövrün içində görüşdülər).

**Faza 2 — dövrün başını tap:**
5. `slow = head` (fast görüş nöqtəsində qalır).
6. Hər ikisini **bir addım** irəlilət.
7. Onlar **dövrün başında** görüşəcəklər.

## Nümunə
`1 → 2 → 3 → 4 → 5 → 3` (3-ə qayıdır)
- Faza 1: slow və fast 4-də görüşür → dövr var.
- Faza 2: slow head-ə qayıdır, 3-də görüşürlər → **dövr başı = 3** ✅

## Kod
```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val, self.next = val, next

def has_cycle(head):
    slow = fast = head
    while fast and fast.next:
        slow = slow.next            # 1 addım
        fast = fast.next.next       # 2 addım
        if slow is fast:            # görüşdülər → dövr
            return True
    return False

def find_cycle_start(head):
    slow = fast = head
    while fast and fast.next:       # Faza 1
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            break
    else:
        return None                 # dövr yoxdur
    slow = head                     # Faza 2
    while slow is not fast:
        slow = slow.next
        fast = fast.next
    return slow                     # dövrün başı

# 1→2→3→4→5→(3-ə qayıt)
nodes = [ListNode(i) for i in range(1, 6)]
for i in range(4):
    nodes[i].next = nodes[i + 1]
nodes[4].next = nodes[2]
print(has_cycle(nodes[0]))                  # True
print(find_cycle_start(nodes[0]).val)       # 3
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(n) |
| Yaddaş | O(1) — əlavə struktur yoxdur |

## Nə vaxt
- ✅ Bağlı siyahıda əlavə yaddaşsız dövr aşkarlama; dövrün başını tapmaq.
- ✅ Massivdə təkrarlanan ədədi tapmaq (funksiya iterasiyasında dövr kimi modelləşir).
- ❌ Yönlü qrafda dövr — 3-rəngli DFS daha ümumidir.
- ❌ Ümumi qraf strukturunda dövr — DFS/Union-Find.

## Əlaqəli
- [DFS](../GraphAlgorithms/DFS.md) — yönlü qraflar üçün dövr aşkarlama.
- [Karger's Algorithm](KargerAlgorithm.md) — həm də təsadüfi/ehtimallı qraf alqoritmi.
