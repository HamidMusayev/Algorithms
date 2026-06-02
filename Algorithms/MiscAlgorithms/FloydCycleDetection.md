# Floyd's Cycle Detection (Tortoise and Hare) 🐢🐇

---

## 🧠 Intuition

Imagine two runners on a circular track. One runs twice as fast as the other. If the track has a loop, the fast runner will eventually lap the slow one — they must meet. Floyd's algorithm applies this to linked lists and any iterated function: use two pointers (slow and fast) and if they ever meet, there's a cycle.

Bonus: after finding the meeting point, you can also find **where the cycle starts** by resetting slow to the head and advancing both one step at a time until they meet again.

**Mental model:** "Fast pointer laps slow pointer in any cycle — their meeting proves the cycle exists. Reset one pointer to find where the cycle begins."

---

## 📊 Complexity

| Metric | Value |
|--------|-------|
| Time Complexity | O(n) |
| Space Complexity | O(1) — no extra data structures |
| Finds cycle start | ✅ Yes, with one extra pass |

---

## ⚙️ How It Works

**Phase 1 — Detect cycle:**
1. Initialize `slow = fast = head`.
2. Move `slow` one step, `fast` two steps per iteration.
3. If `fast` reaches None → **no cycle**.
4. If `slow == fast` → **cycle detected** (they met inside the cycle).

**Phase 2 — Find cycle start:**
5. Reset `slow = head` (fast stays at meeting point).
6. Advance both **one step** at a time.
7. They will meet at the **start of the cycle**.

**Why it works for the start:** Mathematical proof using the distances: if the cycle starts at index `μ` and has length `λ`, the meeting point in phase 2 is exactly at `μ`.

---

## 🔢 Step-by-Step Trace

Linked list: `1 → 2 → 3 → 4 → 5 → 3` (cycle back to node 3)

**Phase 1:**

| Step | slow | fast | slow.val | fast.val |
|------|------|------|----------|----------|
| 0    | 1    | 1    | 1        | 1        |
| 1    | 2    | 3    | 2        | 3        |
| 2    | 3    | 5    | 3        | 5        |
| 3    | 4    | 4    | 4        | 4        |

slow == fast at node 4 → **Cycle detected!**

**Phase 2 (find start):**
- Reset slow to node 1 (head), fast stays at node 4.
- Step 1: slow=2, fast=5
- Step 2: slow=3, fast=3 → **Both at node 3 = cycle start** ✅

---

## 🐍 Python Implementation

```python
class ListNode:
    def __init__(self, val=0, next=None):
        self.val = val
        self.next = next


def has_cycle(head):
    """
    Detect if linked list has a cycle.
    Returns True if cycle exists, False otherwise.
    """
    slow = fast = head
    while fast and fast.next:
        slow = slow.next          # Move slow by 1
        fast = fast.next.next     # Move fast by 2
        if slow is fast:          # They met → cycle!
            return True
    return False                  # fast reached end → no cycle


def find_cycle_start(head):
    """
    Find the node where the cycle begins.
    Returns the cycle-start node, or None if no cycle.
    """
    slow = fast = head

    # Phase 1: Detect cycle
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            break
    else:
        return None    # No cycle

    # Phase 2: Find cycle start
    # Reset slow to head; keep fast at meeting point
    slow = head
    while slow is not fast:
        slow = slow.next    # Both move one step at a time
        fast = fast.next
    return slow             # They meet at the cycle start


def cycle_length(head):
    """Find the length of the cycle (number of nodes in the cycle)."""
    slow = fast = head
    while fast and fast.next:
        slow = slow.next
        fast = fast.next.next
        if slow is fast:
            # Count cycle length
            length = 1
            current = slow.next
            while current is not slow:
                length += 1
                current = current.next
            return length
    return 0


# Example: Build list 1 → 2 → 3 → 4 → 5 → (back to 3)
nodes = [ListNode(i) for i in range(1, 6)]
for i in range(len(nodes) - 1):
    nodes[i].next = nodes[i + 1]
nodes[4].next = nodes[2]   # Create cycle: 5 → 3

head = nodes[0]
print("Has cycle:", has_cycle(head))               # True
start = find_cycle_start(head)
print("Cycle starts at:", start.val)               # 3
print("Cycle length:", cycle_length(head))         # 3 (nodes 3,4,5)
```

---

## 🎯 Recognize This Problem When...

- You need to detect a **cycle in a linked list** without extra memory.
- The problem asks to find a **duplicate number** in an array (can be modeled as a cycle in function iteration).
- Keywords: "cycle in linked list", "loop detection", "does the list have a cycle?".
- You need to find the **start of the cycle** (entry point of the loop).

---

## ✅ When to Use / ❌ When NOT to Use

| Situation | Verdict |
|-----------|---------|
| Cycle detection in linked list | ✅ O(1) space — classic use case |
| Find duplicate in array 1..n (rho technique) | ✅ Model as function cycle |
| Cycle detection in directed graph | ❌ DFS with 3-color marking is more general |
| Cycle in general graph structure | ❌ Use DFS/Union-Find |

---

## 🔗 Related Algorithms

| Algorithm | How it relates |
|-----------|---------------|
| [DFS](../GraphAlgorithms/DFS.md) | DFS cycle detection for directed graphs |
| [Karger's Algorithm](KargerAlgorithm.md) | Both are randomized/probabilistic graph algorithms |

---

## 📝 Practice Problems

| Problem | Platform | Difficulty |
|---------|----------|------------|
| [Linked List Cycle](https://leetcode.com/problems/linked-list-cycle/) | LeetCode | 🟢 Easy |
| [Linked List Cycle II](https://leetcode.com/problems/linked-list-cycle-ii/) | LeetCode | 🟡 Medium |
| [Find the Duplicate Number](https://leetcode.com/problems/find-the-duplicate-number/) | LeetCode | 🟡 Medium |
| [Happy Number](https://leetcode.com/problems/happy-number/) | LeetCode | 🟢 Easy |
