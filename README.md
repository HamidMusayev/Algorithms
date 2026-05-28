## Comprehensive list of important **algorithms**, categorized by type:

> Every entry links to a focused write-up with a **Python implementation**, time/space complexity, and a "when to use" guide. Kept Python-only for simplicity.

---

### **1. Searching Algorithms** 🔍
- **[Linear Search](Algorithms/SearchAlgorithms/LinearSearch.md)** – O(n), simple but inefficient; checks each element one by one.
- **[Binary Search](Algorithms/SearchAlgorithms/BinarySearch.md)** – O(log n), efficient for sorted arrays; repeatedly divides the search space in half.
- **[Jump Search](Algorithms/SearchAlgorithms/JumpSearch.md)** – O(√n), faster than linear search; works on sorted arrays using fixed-size jumps.
- **[Interpolation Search](Algorithms/SearchAlgorithms/InterpolationSearch.md)** – O(log log n) average case; works efficiently for uniformly distributed sorted data.
- **[Exponential Search](Algorithms/SearchAlgorithms/ExponentialSearch.md)** – O(log n); useful for unbounded/infinite lists; combines binary search with range expansion.

---

### **2. Sorting Algorithms** 🔢
- **[Bubble Sort](Algorithms/SortAlgorithms/BubbleSort.md)** – O(n²), simple but inefficient.
- **[Selection Sort](Algorithms/SortAlgorithms/SelectionSort.md)** – O(n²), selects the smallest element in each pass.
- **[Insertion Sort](Algorithms/SortAlgorithms/InsertionSort.md)** – O(n²), better for small/near-sorted data.
- **[Merge Sort](Algorithms/SortAlgorithms/MergeSort.md)** – O(n log n), divide-and-conquer, stable.
- **[Quick Sort](Algorithms/SortAlgorithms/QuickSort.md)** – O(n log n) average, O(n²) worst; very fast in practice.
- **[Heap Sort](Algorithms/SortAlgorithms/HeapSort.md)** – O(n log n), in-place, not stable.
- **[Counting Sort](Algorithms/SortAlgorithms/CountingSort.md)** – O(n + k), small integer ranges, non-comparison.
- **[Radix Sort](Algorithms/SortAlgorithms/RadixSort.md)** – O(n·k), integers/strings with bounded digit count.
- **[Bucket Sort](Algorithms/SortAlgorithms/BucketSort.md)** – O(n + k) average for uniformly distributed input.
- **[Shell Sort](Algorithms/SortAlgorithms/ShellSort.md)** – generalized insertion sort with diminishing gaps.

---

### **3. Graph Algorithms** 🌐
- **[Depth First Search (DFS)](Algorithms/GraphAlgorithms/DFS.md)** – Recursive or iterative graph traversal.
- **[Breadth First Search (BFS)](Algorithms/GraphAlgorithms/BFS.md)** – Level-order traversal; shortest path in unweighted graphs.
- **[Dijkstra's Algorithm](Algorithms/GraphAlgorithms/Dijkstra.md)** – Shortest path for non-negative weights.
- **[Bellman-Ford Algorithm](Algorithms/GraphAlgorithms/BellmanFord.md)** – Shortest path with negative weights; detects negative cycles.
- **[Floyd-Warshall Algorithm](Algorithms/GraphAlgorithms/FloydWarshall.md)** – All-pairs shortest path.
- **[A* Search Algorithm](Algorithms/GraphAlgorithms/AStar.md)** – Heuristic-guided optimal pathfinding.
- **[Prim's Algorithm](Algorithms/GraphAlgorithms/Prim.md)** – Minimum spanning tree (greedy, grow tree).
- **[Kruskal's Algorithm](Algorithms/GraphAlgorithms/Kruskal.md)** – Minimum spanning tree (edge sorting + DSU).
- **[Topological Sorting](Algorithms/GraphAlgorithms/TopologicalSort.md)** – DAG ordering; Kahn's & DFS variants.
- **[Tarjan's Algorithm](Algorithms/GraphAlgorithms/Tarjan.md)** – Strongly Connected Components in one DFS.
- **[Kosaraju's Algorithm](Algorithms/GraphAlgorithms/Kosaraju.md)** – Strongly Connected Components in two DFS passes.

---

### **4. Dynamic Programming (DP) Algorithms** 📈
- **[Fibonacci Sequence (DP)](Algorithms/DPAlgorithms/Fibonacci.md)**
- **[Knapsack Problem (0/1)](Algorithms/DPAlgorithms/Knapsack.md)**
- **[Longest Common Subsequence (LCS)](Algorithms/DPAlgorithms/LCS.md)**
- **[Longest Increasing Subsequence (LIS)](Algorithms/DPAlgorithms/LIS.md)**
- **[Matrix Chain Multiplication](Algorithms/DPAlgorithms/MatrixChainMultiplication.md)**
- **[Coin Change Problem](Algorithms/DPAlgorithms/CoinChange.md)**
- **[Edit Distance (Levenshtein)](Algorithms/DPAlgorithms/EditDistance.md)**
- **[Subset Sum Problem](Algorithms/DPAlgorithms/SubsetSum.md)**
- **[Rod Cutting Problem](Algorithms/DPAlgorithms/RodCutting.md)**

---

### **5. Greedy Algorithms** 💰
- **[Huffman Coding](Algorithms/GreedyAlgorithms/HuffmanCoding.md)** – Data compression.
- **[Activity Selection Problem](Algorithms/GreedyAlgorithms/ActivitySelection.md)** – Scheduling activities.
- **[Fractional Knapsack](Algorithms/GreedyAlgorithms/FractionalKnapsack.md)** – Maximize profit with fractional items.
- **[Job Scheduling with Deadlines](Algorithms/GreedyAlgorithms/JobScheduling.md)**
- **[Dijkstra's Algorithm](Algorithms/GraphAlgorithms/Dijkstra.md)** – Also a greedy approach.
- **[Prim's Algorithm](Algorithms/GraphAlgorithms/Prim.md)** – Also uses greedy strategy.

---

### **6. Backtracking Algorithms** 🔄
- **[N-Queens Problem](Algorithms/BacktrackingAlgorithms/NQueens.md)**
- **[Sudoku Solver](Algorithms/BacktrackingAlgorithms/SudokuSolver.md)**
- **[Knight's Tour Problem](Algorithms/BacktrackingAlgorithms/KnightsTour.md)**
- **[Hamiltonian Cycle](Algorithms/BacktrackingAlgorithms/HamiltonianCycle.md)**
- **[Subset Sum (Backtracking)](Algorithms/BacktrackingAlgorithms/SubsetSumBacktracking.md)**
- **[Word Break Problem](Algorithms/BacktrackingAlgorithms/WordBreak.md)**

---

### **7. String Algorithms** 🧵
- **[KMP Algorithm (Knuth-Morris-Pratt)](Algorithms/StringAlgorithms/KMP.md)** – Efficient substring search.
- **[Rabin-Karp Algorithm](Algorithms/StringAlgorithms/RabinKarp.md)** – Rolling-hash pattern matching.
- **[Z-Algorithm](Algorithms/StringAlgorithms/ZAlgorithm.md)** – Linear-time prefix matching.
- **[Boyer-Moore Algorithm](Algorithms/StringAlgorithms/BoyerMoore.md)** – Fast right-to-left pattern matching.
- **[Aho-Corasick Algorithm](Algorithms/StringAlgorithms/AhoCorasick.md)** – Multi-pattern search.
- **[Suffix Array & Suffix Tree](Algorithms/StringAlgorithms/SuffixArray.md)** – Fast substring queries.

---

### **8. Number Theory Algorithms** 🔢
- **[GCD — Euclidean Algorithm](Algorithms/NumberTheoryAlgorithms/GCD.md)**
- **[Least Common Multiple (LCM)](Algorithms/NumberTheoryAlgorithms/LCM.md)**
- **[Sieve of Eratosthenes](Algorithms/NumberTheoryAlgorithms/SieveOfEratosthenes.md)** – Fast prime generation.
- **[Modular Exponentiation](Algorithms/NumberTheoryAlgorithms/ModularExponentiation.md)** – Fast `a^b mod m`.
- **[Chinese Remainder Theorem](Algorithms/NumberTheoryAlgorithms/ChineseRemainderTheorem.md)**
- **[Fermat's Little Theorem](Algorithms/NumberTheoryAlgorithms/FermatTheorem.md)** – Inverses & primality.
- **[Miller-Rabin Primality Test](Algorithms/NumberTheoryAlgorithms/MillerRabin.md)** – Fast/strong primality.

---

### **9. Computational Geometry Algorithms** 📏
- **[Convex Hull](Algorithms/GeometryAlgorithms/ConvexHull.md)** – Graham's Scan, Jarvis March, Andrew's Chain.
- **[Line Intersection Algorithms](Algorithms/GeometryAlgorithms/LineIntersection.md)**
- **[Closest Pair of Points](Algorithms/GeometryAlgorithms/ClosestPairOfPoints.md)**
- **[Voronoi Diagram](Algorithms/GeometryAlgorithms/VoronoiDiagram.md)**
- **[Sweep Line Algorithm](Algorithms/GeometryAlgorithms/SweepLine.md)**

---

### **10. Miscellaneous Algorithms** 🛠
- **[Reservoir Sampling](Algorithms/MiscAlgorithms/ReservoirSampling.md)** – Random sampling from a stream.
- **[Floyd's Cycle Detection](Algorithms/MiscAlgorithms/FloydCycleDetection.md)** – Tortoise & hare.
- **[Karger's Algorithm](Algorithms/MiscAlgorithms/KargerAlgorithm.md)** – Randomized min-cut.
- **[Hopcroft-Karp Algorithm](Algorithms/MiscAlgorithms/HopcroftKarp.md)** – Maximum bipartite matching.
- **[Monte Carlo & Las Vegas](Algorithms/MiscAlgorithms/MonteCarloLasVegas.md)** – Randomized algorithms.
- **[PageRank Algorithm](Algorithms/MiscAlgorithms/PageRank.md)** – Web/citation graph ranking.

---

### **11. Machine Learning & AI Algorithms** 🤖
- **[Gradient Descent](Algorithms/MLAlgorithms/GradientDescent.md)** – The workhorse optimizer.
- **[Backpropagation](Algorithms/MLAlgorithms/Backpropagation.md)** – Training neural networks.
- **[Decision Trees](Algorithms/MLAlgorithms/DecisionTrees.md)** – Supervised learning.
- **[Random Forest](Algorithms/MLAlgorithms/RandomForest.md)** – Bagged tree ensemble.
- **[K-Means Clustering](Algorithms/MLAlgorithms/KMeansClustering.md)** – Unsupervised partitioning.
- **[Support Vector Machine (SVM)](Algorithms/MLAlgorithms/SVM.md)** – Max-margin classification.

---
