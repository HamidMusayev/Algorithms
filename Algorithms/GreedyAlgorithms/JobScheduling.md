# Job Scheduling with Deadlines 🗓️

---

## 🧠 Intuition

You have n freelance jobs, each taking exactly 1 day. Each job has a profit and a deadline (must finish by day `d`). You can only do one job per day. You want to maximize total profit.

The greedy strategy: process jobs in **decreasing order of profit**. For each job, assign it to the **latest available day slot** before its deadline. This preserves early slots for future (possibly lower-profit) jobs.

**Mental model:** "Best-paying job first, take the latest available slot before its deadline."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n log n + n × d) — naive slot search |
| Space Complexity | O(d) for time slots |
| Greedy choice | Highest profit first |

> With a Disjoint Set Union (DSU), the "find latest free slot" step can be made nearly O(1) amortized, giving overall O(n log n).

---

## ⚙️ How It Works

1. **Sort jobs** by profit in **descending** order.
2. Create a `slots` array of size `max_deadline + 1`, initialized to None (all free).
3. For each job (in profit order):
   - Find the **latest free slot** ≤ the job's deadline.
   - If found: assign the job to that slot, add its profit.
   - If not found: skip (no slot available before deadline).
4. Return selected jobs and total profit.

---

## 🔢 Step-by-Step Trace

Jobs: `[('a', dl=2, $100), ('b', dl=1, $19), ('c', dl=2, $27), ('d', dl=1, $25), ('e', dl=3, $15)]`

Sorted by profit: a($100), c($27), d($25), b($19), e($15)

| Job | Deadline | Latest free slot ≤ dl | Assigned slot | Running profit |
|-----|----------|----------------------|---------------|----------------|
| a   | 2        | slot 2 → free        | slot 2        | $100           |
| c   | 2        | slot 2 → taken, slot 1 → free | slot 1 | $127        |
| d   | 1        | slot 1 → taken, no free slot ≤ 1 | skip | $127       |
| b   | 1        | no free slot ≤ 1     | skip          | $127           |
| e   | 3        | slot 3 → free        | slot 3        | $142           |

Selected: **c, a, e** — total **$142** ✅

---

## 🐍 Python Implementation

```python
def job_scheduling(jobs):
    """
    jobs: list of (job_id, deadline, profit)
    Returns (selected_jobs, total_profit)
    """
    # Sort by profit descending — greedy choice: highest profit first
    jobs = sorted(jobs, key=lambda x: x[2], reverse=True)
    max_deadline = max(j[1] for j in jobs)
    slots = [None] * (max_deadline + 1)   # slots[t] = job assigned to day t
    total_profit = 0

    for jid, dl, profit in jobs:
        # Find latest free slot ≤ deadline (search backwards from deadline)
        for t in range(min(dl, max_deadline), 0, -1):
            if slots[t] is None:
                slots[t] = jid
                total_profit += profit
                break              # Assigned — move to next job

    selected = [(t, slots[t]) for t in range(1, max_deadline + 1) if slots[t]]
    return selected, total_profit


class DSU:
    """Union-Find for fast 'find latest free slot' in O(α(n)) per operation."""
    def __init__(self, n):
        self.parent = list(range(n + 1))

    def find(self, x):
        if self.parent[x] != x:
            self.parent[x] = self.find(self.parent[x])
        return self.parent[x]

    def union_with_prev(self, x):
        """After using slot x, point it to x-1 (so next find skips x)."""
        self.parent[x] = x - 1


def job_scheduling_fast(jobs):
    """O(n log n) version using DSU for fast slot lookup."""
    jobs = sorted(jobs, key=lambda x: x[2], reverse=True)
    max_deadline = max(j[1] for j in jobs)
    dsu = DSU(max_deadline)
    selected, total_profit = [], 0

    for jid, dl, profit in jobs:
        slot = dsu.find(dl)        # Find the latest free slot ≤ dl
        if slot > 0:
            selected.append((slot, jid))
            total_profit += profit
            dsu.union_with_prev(slot)   # Mark slot as used

    return sorted(selected), total_profit


# Example
jobs = [('a', 2, 100), ('b', 1, 19), ('c', 2, 27), ('d', 1, 25), ('e', 3, 15)]
selected, profit = job_scheduling(jobs)
print("Selected:", selected)    # Day 1: c, Day 2: a, Day 3: e
print("Profit:", profit)        # 142
```

---

## 🎯 Recognize This Problem When...

- Each job takes exactly **one unit of time** and must finish **by its deadline**.
- You want to **maximize total profit** from scheduled jobs.
- Keywords: "jobs with deadlines", "select tasks to maximize profit", "1-unit-time jobs".
- Each time slot can hold exactly one job.

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Unit-time jobs with deadlines and profits | ✅ Classic application |
| CPU task scheduling (1 process per cycle) | ✅ Direct model |
| Jobs have varying durations | ❌ Greedy fails — use more complex scheduling |
| Maximize count of jobs (not profit) | ❌ Use Activity Selection instead |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [Activity Selection](ActivitySelection.md) | Maximizes count of non-overlapping activities |
| [Fractional Knapsack](FractionalKnapsack.md) | Also greedy: ratio-based selection |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Job Sequencing Problem](https://www.geeksforgeeks.org/job-sequencing-problem/) | GeeksForGeeks | 🟡 Medium |
| [Maximum Profit in Job Scheduling](https://leetcode.com/problems/maximum-profit-in-job-scheduling/) | LeetCode | 🔴 Hard |
| [Two City Scheduling](https://leetcode.com/problems/two-city-scheduling/) | LeetCode | 🟡 Medium |
