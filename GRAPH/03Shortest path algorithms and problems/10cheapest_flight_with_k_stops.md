# LeetCode 787 - Cheapest Flights Within K Stops

## Problem
- Given `n` cities and flight edges `(u → v, price)`.
- Find **minimum cost** from `src` to `dst` with at most `k` stops.
- If no path exists, return `-1`.

---

## Key Idea: Level traversal (Stops) instead of Pure Cost
- In **normal Dijkstra** → always expand **lowest cumulative cost** first.
- In **this problem** → path validity also depends on **number of stops ≤ k**.
- A *low-cost* path with many stops may be **invalid**.
- So:
    - We traverse **layer by layer (BFS on stops)**.
    - We still track **minimum cost to each node**, but only update if:
        - `newCost < cost[node]`
        - `currentLevel < k+1` (since level counts nodes visited, stops = level-1).

---

## Why Queue works instead of PriorityQueue
- Each edge traversal **always moves to the next level (stop+1)**.
- BFS naturally processes nodes in non-decreasing order of stops.
- Since we **never revisit a node at a lower stop count**, a regular `Queue` (FIFO) works.
- `PriorityQueue` also works but isn't required because levels increase uniformly.

---

## Algorithm
1. Build adjacency list: `adj[u] → (v, price)`.
2. Initialize `cost[]` to `∞`, except `cost[src]=0`.
3. Push `(src, 0, 0)` → `(node, costSoFar, level)` into queue.
4. While queue not empty:
    - Pop `(node, currCost, level)`.
    - If `level > k` → skip.
    - For each neighbor `(nbr, edgeCost)`:
        - If `currCost + edgeCost < cost[nbr]` **and** `level ≤ k`:
            - Update `cost[nbr]`.
            - Push `(nbr, newCost, level+1)`.
5. Return `cost[dst]` or `-1` if unchanged.

---

## Time Complexity
- With `PriorityQueue`, would be **O(E log V)** but `Queue` suffices.
- with normal queue we remove log V , TC=O(E)



## Why not just Dijkstra?
- Dijkstra only cares about **cost**, not number of stops.
- A very cheap path with many stops would incorrectly dominate the queue.
- Level-order traversal enforces the `≤ k stops` constraint naturally.

---

## Code Reference (Java)
```java
class Solution {
    public int findCheapestPrice(int n, int[][] flights, int src, int dst, int k) {
        List<List<Pair>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
        for (int[] f : flights) adj.get(f[0]).add(new Pair(f[1], f[2]));

        Queue<Triplet> q = new LinkedList<>();
        int[] cost = new int[n];
        Arrays.fill(cost, Integer.MAX_VALUE);
        cost[src] = 0;
        q.add(new Triplet(src, 0, 0)); // (node, costSoFar, level)

        while (!q.isEmpty()) {
            Triplet t = q.poll();
            int node = t.first, currCost = t.second, level = t.third;

            if (level > k) continue;

            for (Pair p : adj.get(node)) {
                int nbr = p.first, price = p.second;
                if (currCost + price < cost[nbr]) {
                    cost[nbr] = currCost + price;
                    q.add(new Triplet(nbr, cost[nbr], level + 1));
                }
            }
        }
        return cost[dst] == Integer.MAX_VALUE ? -1 : cost[dst];
    }
}
