# Algorithms Learning Repository 📚

> Every entry links to a focused write-up with: **intuition + analogy**, **step-by-step trace**, **commented Python implementation**, **complexity table**, **when to use**, **related algorithms**, and **practice problems**.

---

## 🗺️ Learning Path

Not sure where to start? Follow this progression from beginner to advanced.

### 🟢 Beginner — Core Foundations
Start here. These algorithms appear in almost every interview and form the mental models you'll use everywhere.

| Algorithm | Why First |
|-----------|-----------|
| [Linear Search](Algorithms/SearchAlgorithms/LinearSearch.md) | Simplest possible search — the baseline |
| [Binary Search](Algorithms/SearchAlgorithms/BinarySearch.md) | Most important search; used everywhere |
| [Bubble Sort](Algorithms/SortAlgorithms/BubbleSort.md) | Simplest sort; teaches the comparison mindset |
| [Selection Sort](Algorithms/SortAlgorithms/SelectionSort.md) | Teaches "find minimum" pattern |
| [Insertion Sort](Algorithms/SortAlgorithms/InsertionSort.md) | Adaptive; used inside TimSort (Python's sort) |
| [Merge Sort](Algorithms/SortAlgorithms/MergeSort.md) | First O(n log n) — teaches divide and conquer |
| [Quick Sort](Algorithms/SortAlgorithms/QuickSort.md) | Fastest in practice; teaches partitioning |
| [BFS](Algorithms/GraphAlgorithms/BFS.md) | Level-order traversal; shortest path |
| [DFS](Algorithms/GraphAlgorithms/DFS.md) | Depth-first exploration; backtracking foundation |
| [Fibonacci (DP)](Algorithms/DPAlgorithms/Fibonacci.md) | First DP — teaches memoization |

---

### 🟡 Intermediate — Interview Essentials
These topics cover 80% of algorithm interview questions at top tech companies.

| Algorithm | Why Important |
|-----------|---------------|
| [Heap Sort](Algorithms/SortAlgorithms/HeapSort.md) | Teaches the heap data structure |
| [Dijkstra](Algorithms/GraphAlgorithms/Dijkstra.md) | Weighted shortest path — classic interview problem |
| [Topological Sort](Algorithms/GraphAlgorithms/TopologicalSort.md) | DAG ordering; course scheduling, build systems |
| [0/1 Knapsack](Algorithms/DPAlgorithms/Knapsack.md) | Cornerstone DP pattern |
| [LCS](Algorithms/DPAlgorithms/LCS.md) | 2D DP — diff tools, DNA alignment |
| [LIS](Algorithms/DPAlgorithms/LIS.md) | Classic DP + binary search optimization |
| [Coin Change](Algorithms/DPAlgorithms/CoinChange.md) | Unbounded knapsack pattern |
| [Edit Distance](Algorithms/DPAlgorithms/EditDistance.md) | Spell checkers, fuzzy matching |
| [N-Queens](Algorithms/BacktrackingAlgorithms/NQueens.md) | Backtracking template — try, check, undo |
| [KMP](Algorithms/StringAlgorithms/KMP.md) | O(n+m) string search |
| [Activity Selection](Algorithms/GreedyAlgorithms/ActivitySelection.md) | Greedy interval scheduling |
| [GCD / LCM](Algorithms/NumberTheoryAlgorithms/GCD.md) | Number theory foundation |
| [Floyd's Cycle Detection](Algorithms/MiscAlgorithms/FloydCycleDetection.md) | Classic linked list interview problem |

---

### 🔴 Advanced — Competitive Programming & Depth
For competitive programming, system design, and deep algorithmic knowledge.

| Algorithm | Why Advanced |
|-----------|--------------|
| [Bellman-Ford](Algorithms/GraphAlgorithms/BellmanFord.md) | Negative weights; cycle detection |
| [Floyd-Warshall](Algorithms/GraphAlgorithms/FloydWarshall.md) | All-pairs shortest paths |
| [A* Search](Algorithms/GraphAlgorithms/AStar.md) | Heuristic pathfinding — game AI, GPS |
| [Prim's / Kruskal's](Algorithms/GraphAlgorithms/Prim.md) | Minimum Spanning Tree |
| [Tarjan / Kosaraju](Algorithms/GraphAlgorithms/Tarjan.md) | Strongly Connected Components |
| [Matrix Chain Multiplication](Algorithms/DPAlgorithms/MatrixChainMultiplication.md) | Interval DP pattern |
| [Huffman Coding](Algorithms/GreedyAlgorithms/HuffmanCoding.md) | Data compression, priority queues |
| [Aho-Corasick](Algorithms/StringAlgorithms/AhoCorasick.md) | Multi-pattern string search |
| [Suffix Array](Algorithms/StringAlgorithms/SuffixArray.md) | Advanced string queries |
| [Sieve of Eratosthenes](Algorithms/NumberTheoryAlgorithms/SieveOfEratosthenes.md) | Prime generation |
| [Miller-Rabin](Algorithms/NumberTheoryAlgorithms/MillerRabin.md) | Large number primality |
| [Modular Exponentiation](Algorithms/NumberTheoryAlgorithms/ModularExponentiation.md) | Cryptography foundation |
| [Convex Hull](Algorithms/GeometryAlgorithms/ConvexHull.md) | Computational geometry |
| [Reservoir Sampling](Algorithms/MiscAlgorithms/ReservoirSampling.md) | Streaming algorithms |
| [Hopcroft-Karp](Algorithms/MiscAlgorithms/HopcroftKarp.md) | Bipartite matching |

---

## 📖 What Each File Contains

Every algorithm page has these sections:

| Section | What You Get |
|---------|--------------|
| 🧠 **Intuition** | Real-world analogy + mental model |
| 📊 **Complexity** | Time/space table with explanatory notes |
| ⚙️ **How It Works** | Clear numbered steps (no code) |
| 🔢 **Step-by-Step Trace** | Algorithm running on a concrete example |
| 🐍 **Python Implementation** | Working code with inline comments |
| 🎯 **Recognize This Problem When** | Signals that tell you to use this algorithm |
| ✅ **When to Use / ❌ When NOT to Use** | Decision table |
| 🔗 **Related Algorithms** | Links to conceptually connected algorithms |
| 📝 **Practice Problems** | LeetCode / HackerRank links with difficulty |

---

## 📂 Algorithm Index

### **1. Searching Algorithms** 🔍
- **[Linear Search](Algorithms/SearchAlgorithms/LinearSearch.md)** – O(n); no sorting required; checks each element.
- **[Binary Search](Algorithms/SearchAlgorithms/BinarySearch.md)** – O(log n); requires sorted array; halves search space each step.
- **[Jump Search](Algorithms/SearchAlgorithms/JumpSearch.md)** – O(√n); sorted arrays; jumps in fixed blocks then linear scan.
- **[Interpolation Search](Algorithms/SearchAlgorithms/InterpolationSearch.md)** – O(log log n) average; best for uniformly distributed data.
- **[Exponential Search](Algorithms/SearchAlgorithms/ExponentialSearch.md)** – O(log n); great for unbounded/infinite sorted lists.

---

### **2. Sorting Algorithms** 🔢
- **[Bubble Sort](Algorithms/SortAlgorithms/BubbleSort.md)** – O(n²); stable; adaptive; good for teaching.
- **[Selection Sort](Algorithms/SortAlgorithms/SelectionSort.md)** – O(n²); not stable; minimizes swaps.
- **[Insertion Sort](Algorithms/SortAlgorithms/InsertionSort.md)** – O(n²); stable; adaptive; best for small/nearly-sorted data.
- **[Merge Sort](Algorithms/SortAlgorithms/MergeSort.md)** – O(n log n); stable; guaranteed; needs O(n) space.
- **[Quick Sort](Algorithms/SortAlgorithms/QuickSort.md)** – O(n log n) average; in-place; fastest in practice.
- **[Heap Sort](Algorithms/SortAlgorithms/HeapSort.md)** – O(n log n); in-place; not stable; guaranteed worst case.
- **[Counting Sort](Algorithms/SortAlgorithms/CountingSort.md)** – O(n + k); non-comparison; beats O(n log n) for small integers.
- **[Radix Sort](Algorithms/SortAlgorithms/RadixSort.md)** – O(n × d); non-comparison; great for fixed-width integers.
- **[Bucket Sort](Algorithms/SortAlgorithms/BucketSort.md)** – O(n + k) average; best for uniformly distributed floats.
- **[Shell Sort](Algorithms/SortAlgorithms/ShellSort.md)** – O(n^1.5) average; in-place; faster than O(n²) sorts.

---

### **3. Graph Algorithms** 🌐
- **[BFS](Algorithms/GraphAlgorithms/BFS.md)** – O(V+E); level-by-level; shortest path in unweighted graphs.
- **[DFS](Algorithms/GraphAlgorithms/DFS.md)** – O(V+E); depth-first; cycle detection, connected components.
- **[Dijkstra](Algorithms/GraphAlgorithms/Dijkstra.md)** – O((V+E) log V); shortest path for non-negative weights.
- **[Bellman-Ford](Algorithms/GraphAlgorithms/BellmanFord.md)** – O(VE); handles negative weights; detects negative cycles.
- **[Floyd-Warshall](Algorithms/GraphAlgorithms/FloydWarshall.md)** – O(V³); all-pairs shortest paths.
- **[A* Search](Algorithms/GraphAlgorithms/AStar.md)** – O(E); heuristic-guided shortest path to a specific goal.
- **[Prim's Algorithm](Algorithms/GraphAlgorithms/Prim.md)** – O((V+E) log V); minimum spanning tree; grows tree.
- **[Kruskal's Algorithm](Algorithms/GraphAlgorithms/Kruskal.md)** – O(E log E); minimum spanning tree; sorts edges.
- **[Topological Sort](Algorithms/GraphAlgorithms/TopologicalSort.md)** – O(V+E); DAG ordering; dependency resolution.
- **[Tarjan's Algorithm](Algorithms/GraphAlgorithms/Tarjan.md)** – O(V+E); strongly connected components in one DFS.
- **[Kosaraju's Algorithm](Algorithms/GraphAlgorithms/Kosaraju.md)** – O(V+E); SCCs in two DFS passes; easier to understand.

---

### **4. Dynamic Programming** 📈
- **[Fibonacci](Algorithms/DPAlgorithms/Fibonacci.md)** – Classic intro to memoization and tabulation.
- **[0/1 Knapsack](Algorithms/DPAlgorithms/Knapsack.md)** – Foundational subset-selection DP.
- **[LCS](Algorithms/DPAlgorithms/LCS.md)** – Longest common subsequence; 2D DP table.
- **[LIS](Algorithms/DPAlgorithms/LIS.md)** – Longest increasing subsequence; O(n log n) with binary search.
- **[Matrix Chain Multiplication](Algorithms/DPAlgorithms/MatrixChainMultiplication.md)** – Interval DP; optimal parenthesization.
- **[Coin Change](Algorithms/DPAlgorithms/CoinChange.md)** – Unbounded knapsack; count ways or minimize coins.
- **[Edit Distance](Algorithms/DPAlgorithms/EditDistance.md)** – Levenshtein distance; insert/delete/replace.
- **[Subset Sum](Algorithms/DPAlgorithms/SubsetSum.md)** – Boolean DP; feasibility check.
- **[Rod Cutting](Algorithms/DPAlgorithms/RodCutting.md)** – Unbounded knapsack; maximize revenue.

---

### **5. Greedy Algorithms** 💰
- **[Huffman Coding](Algorithms/GreedyAlgorithms/HuffmanCoding.md)** – Optimal prefix-free compression; min-heap.
- **[Activity Selection](Algorithms/GreedyAlgorithms/ActivitySelection.md)** – Maximum non-overlapping intervals.
- **[Fractional Knapsack](Algorithms/GreedyAlgorithms/FractionalKnapsack.md)** – Value/weight ratio greedy.
- **[Job Scheduling](Algorithms/GreedyAlgorithms/JobScheduling.md)** – Unit-time jobs; maximize profit by deadline.

---

### **6. Backtracking Algorithms** 🔄
- **[N-Queens](Algorithms/BacktrackingAlgorithms/NQueens.md)** – Place queens without conflict; template problem.
- **[Sudoku Solver](Algorithms/BacktrackingAlgorithms/SudokuSolver.md)** – Constraint satisfaction on a 9×9 grid.
- **[Knight's Tour](Algorithms/BacktrackingAlgorithms/KnightsTour.md)** – Hamiltonian path on a chessboard.
- **[Hamiltonian Cycle](Algorithms/BacktrackingAlgorithms/HamiltonianCycle.md)** – Visit every vertex once and return; NP-complete.
- **[Subset Sum (Backtracking)](Algorithms/BacktrackingAlgorithms/SubsetSumBacktracking.md)** – Enumerate all valid subsets.
- **[Word Break](Algorithms/BacktrackingAlgorithms/WordBreak.md)** – Segment string into dictionary words.

---

### **7. String Algorithms** 🧵
- **[KMP](Algorithms/StringAlgorithms/KMP.md)** – O(n+m); failure function avoids re-comparisons.
- **[Rabin-Karp](Algorithms/StringAlgorithms/RabinKarp.md)** – Rolling hash; excellent for multi-pattern of equal length.
- **[Z-Algorithm](Algorithms/StringAlgorithms/ZAlgorithm.md)** – O(n+m); simpler than KMP; prefix matching.
- **[Boyer-Moore](Algorithms/StringAlgorithms/BoyerMoore.md)** – Sublinear average; fastest for large alphabets.
- **[Aho-Corasick](Algorithms/StringAlgorithms/AhoCorasick.md)** – Multi-pattern search in one text pass.
- **[Suffix Array](Algorithms/StringAlgorithms/SuffixArray.md)** – All substring queries after O(n log n) build.

---

### **8. Number Theory** 🔢
- **[GCD](Algorithms/NumberTheoryAlgorithms/GCD.md)** – Euclidean algorithm; O(log min(a,b)).
- **[LCM](Algorithms/NumberTheoryAlgorithms/LCM.md)** – Computed from GCD; cycle synchronization.
- **[Sieve of Eratosthenes](Algorithms/NumberTheoryAlgorithms/SieveOfEratosthenes.md)** – O(n log log n); all primes up to n.
- **[Modular Exponentiation](Algorithms/NumberTheoryAlgorithms/ModularExponentiation.md)** – O(log exp); cryptography foundation.
- **[Chinese Remainder Theorem](Algorithms/NumberTheoryAlgorithms/ChineseRemainderTheorem.md)** – Simultaneous congruences.
- **[Fermat's Little Theorem](Algorithms/NumberTheoryAlgorithms/FermatTheorem.md)** – Modular inverse for prime modulus.
- **[Miller-Rabin](Algorithms/NumberTheoryAlgorithms/MillerRabin.md)** – Probabilistic primality; no Carmichael failures.

---

### **9. Computational Geometry** 📏
- **[Convex Hull](Algorithms/GeometryAlgorithms/ConvexHull.md)** – Outermost boundary of a point cloud.
- **[Line Intersection](Algorithms/GeometryAlgorithms/LineIntersection.md)** – O(1) orientation test; segment crossing.
- **[Closest Pair of Points](Algorithms/GeometryAlgorithms/ClosestPairOfPoints.md)** – O(n log n) divide and conquer.
- **[Voronoi Diagram](Algorithms/GeometryAlgorithms/VoronoiDiagram.md)** – Nearest-site regions; Fortune's sweep line.
- **[Sweep Line](Algorithms/GeometryAlgorithms/SweepLine.md)** – General paradigm; skyline, intersections, rectangle area.

---

### **10. Miscellaneous** 🛠
- **[Reservoir Sampling](Algorithms/MiscAlgorithms/ReservoirSampling.md)** – Uniform random sample from a stream of unknown length.
- **[Floyd's Cycle Detection](Algorithms/MiscAlgorithms/FloydCycleDetection.md)** – Tortoise & hare; O(1) space cycle detection.
- **[Karger's Algorithm](Algorithms/MiscAlgorithms/KargerAlgorithm.md)** – Randomized minimum cut.
- **[Hopcroft-Karp](Algorithms/MiscAlgorithms/HopcroftKarp.md)** – O(E√V) maximum bipartite matching.
- **[Monte Carlo & Las Vegas](Algorithms/MiscAlgorithms/MonteCarloLasVegas.md)** – Two flavors of randomized algorithms.
- **[PageRank](Algorithms/MiscAlgorithms/PageRank.md)** – Random walk–based graph importance scoring.

---

### **11. Machine Learning & AI** 🤖
- **[Gradient Descent](Algorithms/MLAlgorithms/GradientDescent.md)** – The optimizer behind all ML models.
- **[Backpropagation](Algorithms/MLAlgorithms/Backpropagation.md)** – Gradient computation for neural networks via chain rule.
- **[Decision Trees](Algorithms/MLAlgorithms/DecisionTrees.md)** – Interpretable split-based classifier/regressor.
- **[Random Forest](Algorithms/MLAlgorithms/RandomForest.md)** – Ensemble of diverse trees; robust baseline.
- **[K-Means Clustering](Algorithms/MLAlgorithms/KMeansClustering.md)** – Unsupervised centroid-based clustering.
- **[SVM](Algorithms/MLAlgorithms/SVM.md)** – Maximum-margin classifier; kernel trick for non-linear data.

---

## 🔢 Quick Complexity Reference

| Algorithm | Best | Average | Worst | Space |
|-----------|------|---------|-------|-------|
| Linear Search | O(1) | O(n) | O(n) | O(1) |
| Binary Search | O(1) | O(log n) | O(log n) | O(1) |
| Bubble Sort | O(n) | O(n²) | O(n²) | O(1) |
| Merge Sort | O(n log n) | O(n log n) | O(n log n) | O(n) |
| Quick Sort | O(n log n) | O(n log n) | O(n²) | O(log n) |
| Heap Sort | O(n log n) | O(n log n) | O(n log n) | O(1) |
| BFS / DFS | O(V+E) | O(V+E) | O(V+E) | O(V) |
| Dijkstra | — | O((V+E) log V) | O((V+E) log V) | O(V) |
| KMP | O(n+m) | O(n+m) | O(n+m) | O(m) |

---
