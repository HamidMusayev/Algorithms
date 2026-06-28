# Alqoritmlər

---

## 🗺️ Öyrənmə Yolu

Haradan başlayacağını bilmirsən? Bu ardıcıllıqla get.

### 🟢 Başlanğıc — Əsas Təməllər
| Alqoritm | Niyə əvvəl |
|----------|-----------|
| [Linear Search](Algorithms/SearchAlgorithms/LinearSearch.md) | Ən sadə axtarış — baza |
| [Binary Search](Algorithms/SearchAlgorithms/BinarySearch.md) | Ən vacib axtarış; hər yerdə işlənir |
| [Bubble Sort](Algorithms/SortAlgorithms/BubbleSort.md) | Ən sadə sıralama; müqayisə düşüncəsi |
| [Merge Sort](Algorithms/SortAlgorithms/MergeSort.md) | İlk O(n log n) — böl-və-idarə et |
| [Quick Sort](Algorithms/SortAlgorithms/QuickSort.md) | Praktikada ən sürətli; bölüşdürmə |
| [BFS](Algorithms/GraphAlgorithms/BFS.md) | Səviyyə-səviyyə gəzinti; ən qısa yol |
| [DFS](Algorithms/GraphAlgorithms/DFS.md) | Dərinə gəzinti; backtracking təməli |
| [Fibonacci (DP)](Algorithms/DPAlgorithms/Fibonacci.md) | İlk DP — memoizasiya |

### 🟡 Orta — Müsahibə Əsasları
| Alqoritm | Niyə vacib |
|----------|-----------|
| [Heap Sort](Algorithms/SortAlgorithms/HeapSort.md) | Topa (heap) strukturu |
| [Dijkstra](Algorithms/GraphAlgorithms/Dijkstra.md) | Çəkili ən qısa yol — klassik sual |
| [Topological Sort](Algorithms/GraphAlgorithms/TopologicalSort.md) | DAG sıralaması; asılılıqlar |
| [0/1 Knapsack](Algorithms/DPAlgorithms/Knapsack.md) | Əsas DP nümunəsi |
| [LCS](Algorithms/DPAlgorithms/LCS.md) | 2D DP — diff, DNA |
| [LIS](Algorithms/DPAlgorithms/LIS.md) | DP + binary axtarış |
| [Coin Change](Algorithms/DPAlgorithms/CoinChange.md) | Limitsiz knapsack nümunəsi |
| [Edit Distance](Algorithms/DPAlgorithms/EditDistance.md) | Orfoqrafiya, qeyri-dəqiq uyğunluq |
| [N-Queens](Algorithms/BacktrackingAlgorithms/NQueens.md) | Backtracking şablonu |
| [KMP](Algorithms/StringAlgorithms/KMP.md) | O(n+m) sətir axtarışı |
| [GCD / LCM](Algorithms/NumberTheoryAlgorithms/GCD.md) | Ədədlər nəzəriyyəsi təməli |
| [Floyd's Cycle Detection](Algorithms/MiscAlgorithms/FloydCycleDetection.md) | Klassik bağlı siyahı sualı |

### 🔴 Qabaqcıl — Yarışma & Dərinlik
| Alqoritm | Niyə qabaqcıl |
|----------|--------------|
| [Bellman-Ford](Algorithms/GraphAlgorithms/BellmanFord.md) | Mənfi çəkilər; dövr aşkarlama |
| [Floyd-Warshall](Algorithms/GraphAlgorithms/FloydWarshall.md) | Bütün cütlər arası ən qısa yol |
| [A* Search](Algorithms/GraphAlgorithms/AStar.md) | Heuristik yol tapma — oyun AI, GPS |
| [Prim / Kruskal](Algorithms/GraphAlgorithms/Prim.md) | Minimal Örtən Ağac |
| [Tarjan / Kosaraju](Algorithms/GraphAlgorithms/Tarjan.md) | Güclü Əlaqəli Komponentlər |
| [Aho-Corasick](Algorithms/StringAlgorithms/AhoCorasick.md) | Çoxnaxışlı sətir axtarışı |
| [Suffix Array](Algorithms/StringAlgorithms/SuffixArray.md) | Qabaqcıl sətir sorğuları |
| [Miller-Rabin](Algorithms/NumberTheoryAlgorithms/MillerRabin.md) | Böyük ədəd ilkinliyi |
| [Convex Hull](Algorithms/GeometryAlgorithms/ConvexHull.md) | Hesablama həndəsəsi |
| [Hopcroft-Karp](Algorithms/MiscAlgorithms/HopcroftKarp.md) | İkihissəli uyğunlaşma |

---

## 📖 Hər Fayl Nə Saxlayır

Hər alqoritm səhifəsində bu bölmələr var:

| Bölmə | Nə alırsan |
|-------|-----------|
| **Fikir** | Real bənzətmə + əsas ideya |
| **Necə işləyir** | Aydın nömrəli addımlar |
| **Nümunə** | Konkret misal üzərində icra |
| **Kod** | Şərhli, işlək Python |
| **Mürəkkəblik** | Vaxt/yaddaş cədvəli |
| **Nə vaxt** | ✅ uyğun / ❌ uyğun olmayan hallar |
| **Əlaqəli** | Konseptual bağlı alqoritmlər |

---

## 📂 Alqoritm İndeksi

### 1. Axtarış 🔍
- **[Linear Search](Algorithms/SearchAlgorithms/LinearSearch.md)** — O(n); sıralama tələb etmir.
- **[Binary Search](Algorithms/SearchAlgorithms/BinarySearch.md)** — O(log n); sıralı massiv; hər addımda yarıya bölür.
- **[Jump Search](Algorithms/SearchAlgorithms/JumpSearch.md)** — O(√n); bloklarla sıçrayır.
- **[Interpolation Search](Algorithms/SearchAlgorithms/InterpolationSearch.md)** — O(log log n) orta; bərabər paylanmış data.
- **[Exponential Search](Algorithms/SearchAlgorithms/ExponentialSearch.md)** — O(log n); sonsuz sıralı siyahılar.

### 2. Sıralama 🔢
- **[Bubble Sort](Algorithms/SortAlgorithms/BubbleSort.md)** — O(n²); stabil; öyrətmə üçün.
- **[Selection Sort](Algorithms/SortAlgorithms/SelectionSort.md)** — O(n²); yer dəyişmələri minimumlaşdırır.
- **[Insertion Sort](Algorithms/SortAlgorithms/InsertionSort.md)** — O(n²); stabil; sıralıya yaxın data üçün yaxşı.
- **[Merge Sort](Algorithms/SortAlgorithms/MergeSort.md)** — O(n log n); stabil; O(n) yaddaş.
- **[Quick Sort](Algorithms/SortAlgorithms/QuickSort.md)** — O(n log n) orta; yerində; praktikada sürətli.
- **[Heap Sort](Algorithms/SortAlgorithms/HeapSort.md)** — O(n log n); yerində; zəmanətli pis hal.
- **[Counting Sort](Algorithms/SortAlgorithms/CountingSort.md)** — O(n + k); müqayisəsiz.
- **[Radix Sort](Algorithms/SortAlgorithms/RadixSort.md)** — O(n × d); sabit uzunluqlu tam ədədlər.
- **[Bucket Sort](Algorithms/SortAlgorithms/BucketSort.md)** — O(n + k) orta; bərabər paylanmış float.
- **[Shell Sort](Algorithms/SortAlgorithms/ShellSort.md)** — ~O(n^1.5); yerində.

### 3. Qraf 🌐
- **[BFS](Algorithms/GraphAlgorithms/BFS.md)** — O(V+E); çəkisizdə ən qısa yol.
- **[DFS](Algorithms/GraphAlgorithms/DFS.md)** — O(V+E); dövr, komponentlər.
- **[Dijkstra](Algorithms/GraphAlgorithms/Dijkstra.md)** — O((V+E) log V); mənfi olmayan çəkilər.
- **[Bellman-Ford](Algorithms/GraphAlgorithms/BellmanFord.md)** — O(VE); mənfi çəkilər; dövr aşkarlama.
- **[Floyd-Warshall](Algorithms/GraphAlgorithms/FloydWarshall.md)** — O(V³); bütün cütlər.
- **[A* Search](Algorithms/GraphAlgorithms/AStar.md)** — heuristik yol tapma.
- **[Prim](Algorithms/GraphAlgorithms/Prim.md)** — O((V+E) log V); MST; ağac böyüdür.
- **[Kruskal](Algorithms/GraphAlgorithms/Kruskal.md)** — O(E log E); MST; tilovları sıralayır.
- **[Topological Sort](Algorithms/GraphAlgorithms/TopologicalSort.md)** — O(V+E); DAG sıralaması.
- **[Tarjan](Algorithms/GraphAlgorithms/Tarjan.md)** — O(V+E); SCC bir DFS-də.
- **[Kosaraju](Algorithms/GraphAlgorithms/Kosaraju.md)** — O(V+E); SCC iki DFS-də.

### 4. Dinamik Proqramlaşdırma 📈
- **[Fibonacci](Algorithms/DPAlgorithms/Fibonacci.md)** — memoizasiya və cədvələ giriş.
- **[0/1 Knapsack](Algorithms/DPAlgorithms/Knapsack.md)** — əsas alt-çoxluq seçimi DP.
- **[LCS](Algorithms/DPAlgorithms/LCS.md)** — ən uzun ortaq alt-ardıcıllıq; 2D DP.
- **[LIS](Algorithms/DPAlgorithms/LIS.md)** — ən uzun artan alt-ardıcıllıq; O(n log n).
- **[Matrix Chain Multiplication](Algorithms/DPAlgorithms/MatrixChainMultiplication.md)** — interval DP.
- **[Coin Change](Algorithms/DPAlgorithms/CoinChange.md)** — limitsiz knapsack.
- **[Edit Distance](Algorithms/DPAlgorithms/EditDistance.md)** — Levenshtein məsafəsi.
- **[Subset Sum](Algorithms/DPAlgorithms/SubsetSum.md)** — məntiqi DP; mümkünlük yoxlaması.
- **[Rod Cutting](Algorithms/DPAlgorithms/RodCutting.md)** — limitsiz knapsack; gəliri maksimum et.

### 5. Acgöz (Greedy) 💰
- **[Huffman Coding](Algorithms/GreedyAlgorithms/HuffmanCoding.md)** — optimal prefiks-azad sıxma.
- **[Activity Selection](Algorithms/GreedyAlgorithms/ActivitySelection.md)** — maksimum üst-üstə düşməyən interval.
- **[Fractional Knapsack](Algorithms/GreedyAlgorithms/FractionalKnapsack.md)** — dəyər/çəki nisbəti.
- **[Job Scheduling](Algorithms/GreedyAlgorithms/JobScheduling.md)** — son tarixli tapşırıqlar; mənfəət.

### 6. Backtracking 🔄
- **[N-Queens](Algorithms/BacktrackingAlgorithms/NQueens.md)** — toqquşmasız vəzirlər; şablon.
- **[Sudoku Solver](Algorithms/BacktrackingAlgorithms/SudokuSolver.md)** — 9×9 məhdudiyyət ödəmə.
- **[Knight's Tour](Algorithms/BacktrackingAlgorithms/KnightsTour.md)** — lövhədə Hamilton yolu.
- **[Hamiltonian Cycle](Algorithms/BacktrackingAlgorithms/HamiltonianCycle.md)** — hər təpəni bir dəfə.
- **[Subset Sum (Backtracking)](Algorithms/BacktrackingAlgorithms/SubsetSumBacktracking.md)** — bütün alt-çoxluqları sadala.
- **[Word Break](Algorithms/BacktrackingAlgorithms/WordBreak.md)** — sətiri sözlərə böl.

### 7. Sətir 🧵
- **[KMP](Algorithms/StringAlgorithms/KMP.md)** — O(n+m); uğursuzluq funksiyası.
- **[Rabin-Karp](Algorithms/StringAlgorithms/RabinKarp.md)** — dırnaqlı heş; çoxnaxış.
- **[Z-Algorithm](Algorithms/StringAlgorithms/ZAlgorithm.md)** — O(n+m); KMP-dən sadə.
- **[Boyer-Moore](Algorithms/StringAlgorithms/BoyerMoore.md)** — orta xəttialtı; böyük əlifba.
- **[Aho-Corasick](Algorithms/StringAlgorithms/AhoCorasick.md)** — bir keçiddə çoxnaxış.
- **[Suffix Array](Algorithms/StringAlgorithms/SuffixArray.md)** — bütün alt-sətir sorğuları.

### 8. Ədədlər Nəzəriyyəsi 🔢
- **[GCD](Algorithms/NumberTheoryAlgorithms/GCD.md)** — Evklid; O(log min(a,b)).
- **[LCM](Algorithms/NumberTheoryAlgorithms/LCM.md)** — GCD-dən hesablanır.
- **[Sieve of Eratosthenes](Algorithms/NumberTheoryAlgorithms/SieveOfEratosthenes.md)** — O(n log log n); n-ə qədər sadələr.
- **[Modular Exponentiation](Algorithms/NumberTheoryAlgorithms/ModularExponentiation.md)** — O(log exp); kriptoqrafiya.
- **[Chinese Remainder Theorem](Algorithms/NumberTheoryAlgorithms/ChineseRemainderTheorem.md)** — eyni anlı konqruenslər.
- **[Fermat's Little Theorem](Algorithms/NumberTheoryAlgorithms/FermatTheorem.md)** — sadə modullu modulyar tərs.
- **[Miller-Rabin](Algorithms/NumberTheoryAlgorithms/MillerRabin.md)** — ehtimallı ilkinlik; Karmaykl səhvi yox.

### 9. Hesablama Həndəsəsi 📏
- **[Convex Hull](Algorithms/GeometryAlgorithms/ConvexHull.md)** — nöqtə buludunun kənar sərhədi.
- **[Line Intersection](Algorithms/GeometryAlgorithms/LineIntersection.md)** — O(1) oriyentasiya testi.
- **[Closest Pair of Points](Algorithms/GeometryAlgorithms/ClosestPairOfPoints.md)** — O(n log n) böl-və-idarə et.
- **[Voronoi Diagram](Algorithms/GeometryAlgorithms/VoronoiDiagram.md)** — ən yaxın site regionları.
- **[Sweep Line](Algorithms/GeometryAlgorithms/SweepLine.md)** — ümumi paradiqma; skyline, kəsişmələr.

### 10. Müxtəlif 🛠
- **[Reservoir Sampling](Algorithms/MiscAlgorithms/ReservoirSampling.md)** — naməlum uzunluqlu axından bərabər seçim.
- **[Floyd's Cycle Detection](Algorithms/MiscAlgorithms/FloydCycleDetection.md)** — tısbağa & dovşan; O(1) yaddaş.
- **[Karger's Algorithm](Algorithms/MiscAlgorithms/KargerAlgorithm.md)** — təsadüfi minimum kəsim.
- **[Hopcroft-Karp](Algorithms/MiscAlgorithms/HopcroftKarp.md)** — O(E√V) maksimum ikihissəli uyğunlaşma.
- **[Monte Carlo & Las Vegas](Algorithms/MiscAlgorithms/MonteCarloLasVegas.md)** — təsadüfi alqoritmlərin iki növü.
- **[PageRank](Algorithms/MiscAlgorithms/PageRank.md)** — təsadüfi gəzişmə ilə qraf əhəmiyyəti.

### 11. Maşın Öyrənməsi & AI 🤖
- **[Gradient Descent](Algorithms/MLAlgorithms/GradientDescent.md)** — bütün ML modellərinin arxasındakı optimallaşdırıcı.
- **[Backpropagation](Algorithms/MLAlgorithms/Backpropagation.md)** — zəncir qaydası ilə neyron şəbəkəsi qradiyenti.
- **[Decision Trees](Algorithms/MLAlgorithms/DecisionTrees.md)** — izah edilə bilən təsnifatçı/reqressor.
- **[Random Forest](Algorithms/MLAlgorithms/RandomForest.md)** — ağac ansamblı; sağlam baza.
- **[K-Means Clustering](Algorithms/MLAlgorithms/KMeansClustering.md)** — sentroid əsaslı klasterləşmə.
- **[SVM](Algorithms/MLAlgorithms/SVM.md)** — maksimum-margin təsnifatçı; kernel hiyləsi.

---

## 🔢 Sürətli Mürəkkəblik Cədvəli

| Alqoritm | Ən yaxşı | Orta | Ən pis | Yaddaş |
|----------|----------|------|--------|--------|
| Linear Search | O(1) | O(n) | O(n) | O(1) |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |
| BFS / DFS | O(V+E) | O(V+E) | O(V+E) | O(V) |
| Dijkstra | — | O((V+E) log V) | O((V+E) log V) | O(V) |
| KMP | O(n+m) | O(n+m) | O(n+m) | O(m) |
