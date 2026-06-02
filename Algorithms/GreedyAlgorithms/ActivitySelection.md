# Activity Selection Problem 📅

---

## 🧠 Intuition

You have a single meeting room and a list of meetings, each with a start and end time. You want to schedule as many meetings as possible without overlap. The greedy insight: **always pick the meeting that ends earliest**. By finishing sooner, you leave maximum room for future meetings.

**Mental model:** Sort by finish time. Always pick the activity that ends soonest and doesn't conflict with the last chosen one.

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n log n) — dominated by sorting |
| Space Complexity | O(1) extra |
| Greedy choice | Earliest finishing time |

---

## ⚙️ How It Works

1. **Sort** all activities by their finish time (ascending).
2. Select the **first activity** (earliest finish).
3. For each subsequent activity:
   - If its start time ≥ the finish time of the last selected activity → **compatible** → select it.
   - Otherwise → skip it (it overlaps).
4. Return all selected activities.

**Why greedy works:** Selecting the earliest-finishing compatible activity always leaves at least as much room as any other choice (exchange argument proof).

---

## 🔢 Step-by-Step Trace

Activities (sorted by finish time):

| Activity | Start | Finish |
|----------|-------|--------|
| A1       | 1     | 4      |
| A2       | 3     | 5      |
| A3       | 0     | 6      |
| A4       | 5     | 7      |
| A5       | 8     | 11     |

| Step | Activity | Start | Finish | last_finish | Compatible? | Selected? |
|------|----------|-------|--------|-------------|-------------|-----------|
| 1    | A1       | 1     | 4      | -∞          | ✅          | ✅        |
| 2    | A2       | 3     | 5      | 4           | 3 < 4 ❌    | ❌        |
| 3    | A3       | 0     | 6      | 4           | 0 < 4 ❌    | ❌        |
| 4    | A4       | 5     | 7      | 4           | 5 ≥ 4 ✅    | ✅        |
| 5    | A5       | 8     | 11     | 7           | 8 ≥ 7 ✅    | ✅        |

Selected: **A1, A4, A5** (3 activities) ✅

---

## 🐍 Python Implementation

```python
def activity_selection(activities):
    """
    activities: list of (start, finish) tuples
    Returns the maximum number of non-overlapping activities and which ones.
    """
    # Sort by finish time — the key greedy decision
    activities = sorted(activities, key=lambda x: x[1])

    selected = [activities[0]]   # Always select the first (earliest finish)
    last_finish = activities[0][1]

    for start, finish in activities[1:]:
        if start >= last_finish:        # Compatible: starts after last selection ends
            selected.append((start, finish))
            last_finish = finish        # Update the finish time of last selected

    return selected


def activity_selection_weighted(activities):
    """
    Weighted version (maximize total weight, not count) requires DP.
    activities: list of (start, finish, weight) tuples
    """
    activities = sorted(activities, key=lambda x: x[1])
    n = len(activities)
    dp = [0] * (n + 1)

    def last_compatible(i):
        """Binary search for the last activity that finishes before activity i starts."""
        lo, hi = 0, i - 1
        while lo <= hi:
            mid = (lo + hi) // 2
            if activities[mid][1] <= activities[i][0]:
                lo = mid + 1
            else:
                hi = mid - 1
        return hi    # Returns -1 if none found

    for i in range(n):
        j = last_compatible(i)
        dp[i + 1] = max(dp[i],                            # Skip activity i
                        (dp[j + 1] if j >= 0 else 0) + activities[i][2])  # Take activity i

    return dp[n]


# Example (unweighted)
activities = [(1, 4), (3, 5), (0, 6), (5, 7), (3, 9), (5, 9), (6, 10), (8, 11)]
print("Selected:", activity_selection(activities))
# [(1, 4), (5, 7), (8, 11)]

# Example (weighted DP)
weighted = [(1, 4, 3), (3, 5, 1), (0, 6, 5), (5, 7, 4), (8, 11, 2)]
print("Max weighted:", activity_selection_weighted(weighted))
# 9
```

---

## 🎯 Recognize This Problem When...

- You need to **schedule the maximum number of non-overlapping intervals/tasks**.
- Keywords: "meeting room scheduling", "maximum activities", "non-overlapping intervals".
- Each task has a start and end time and takes the entire interval.
- You want to maximize COUNT (not weighted value) — greedy works.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Maximize count of non-overlapping activities | ✅ Greedy (sort by end time) |
| Meeting room scheduling | ✅ Classic application |
| Each job takes exactly one time unit | ✅ Use Job Scheduling with deadlines |
| Maximize total profit (not count) | ❌ Use Weighted Interval Scheduling (DP) |
| Activities have varying durations and weights | ❌ Greedy fails — use DP |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Job Scheduling](JobScheduling.md) | Also greedy scheduling; jobs take 1 unit each |
| [Fractional Knapsack](FractionalKnapsack.md) | Also greedy; value/weight ratio instead of finish time |
| [Huffman Coding](HuffmanCoding.md) | Also greedy; priority queue of frequencies |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Non-overlapping Intervals](https://leetcode.com/problems/non-overlapping-intervals/) | LeetCode | 🟡 Medium |
| [Meeting Rooms II](https://leetcode.com/problems/meeting-rooms-ii/) | LeetCode | 🟡 Medium |
| [Minimum Number of Arrows](https://leetcode.com/problems/minimum-number-of-arrows-to-burst-balloons/) | LeetCode | 🟡 Medium |
| [Activity Selection](https://www.hackerrank.com/challenges/activity-selection/problem) | HackerRank | 🟢 Easy |
