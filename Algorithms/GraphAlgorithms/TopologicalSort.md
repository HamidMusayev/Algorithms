# Topological Sort (Topoloji Sıralama)

**Fikir:** Geyinərkən corabdan əvvəl ayaqqabı geyinə bilməzsən. Topoloji sıralama tapşırıqları elə düzür ki, hər ön-şərt ondan asılı tapşırıqdan əvvəl gəlsin. **Yalnız yönlü dövrsüz qraflarda (DAG) işləyir.**

## Necə işləyir
**Kahn alqoritmi (BFS əsaslı):**
1. Hər node üçün **giriş-dərəcəsi** (gələn tilov sayı) hesabla.
2. Giriş-dərəcəsi 0 olan bütün node-ları növbəyə əlavə et (ön-şərtsiz).
3. Növbə boşalana qədər:
   - Node çıxar, nəticəyə əlavə et.
   - Hər qonşunun giriş-dərəcəsini 1 azalt; 0 olduqda növbəyə əlavə et.
4. Nəticədə V-dən az node varsa → **dövr var** (sıralama mümkün deyil).

**DFS əsaslı:** node-un bütün törəmələrini gəzəndən sonra onu stekə yığ; steki tərsinə çevir.

## Nümunə
Tilovlar: `5→2, 5→0, 4→0, 4→1, 2→3, 3→1`
Düzgün sıra: **4 → 5 → 0 → 2 → 3 → 1** ✅

## Kod
```python
from collections import deque, defaultdict

def topological_sort(V, edges):
    """edges: (u, v) — u, v-dən əvvəl gəlməlidir."""
    graph = defaultdict(list)
    in_degree = [0] * V
    for u, v in edges:
        graph[u].append(v)
        in_degree[v] += 1
    queue = deque([i for i in range(V) if in_degree[i] == 0])
    order = []
    while queue:
        node = queue.popleft()
        order.append(node)
        for neighbor in graph[node]:
            in_degree[neighbor] -= 1        # bir ön-şərt ödəndi
            if in_degree[neighbor] == 0:
                queue.append(neighbor)
    if len(order) != V:
        raise ValueError("Qrafda dövr var — sıralama mümkün deyil")
    return order

edges = [(5,2), (5,0), (4,0), (4,1), (2,3), (3,1)]
print(topological_sort(6, edges))   # məs. [4, 5, 0, 2, 3, 1]
```

## Mürəkkəblik
| Metrika | Dəyər |
|---------|-------|
| Vaxt | O(V + E) |
| Yaddaş | O(V) |

Dövr aşkarlama daxildir — dövr = düzgün sıralama yoxdur.

## Nə vaxt
- ✅ Asılılıqlı tapşırıqların sıralanması (kurs cədvəli, build sırası, paket asılılıqları).
- ✅ Məhdudiyyətlər çoxluğunun ödənə bilməsini yoxlamaq (dövr = ziddiyyət).
- ❌ Yönsüz qraf və ya dövrlü qraf — düzgün sıra yoxdur.

## Əlaqəli
- [DFS](DFS.md) — post-order tərs topoloji sıra verir.
- [BFS](BFS.md) — Kahn alqoritmi giriş-dərəcələri üzərində BFS-dir.
- [Tarjan](Tarjan.md) — SCC tapır; hər hansı SCC-də >1 node varsa, dövr var.
