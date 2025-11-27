# GFG: Shortest Path in an Undirected Graph with Unit Weights

## Problem Description
Given an **undirected graph** represented as an adjacency list, find the shortest distance from a **source node** to all other nodes.  
All edges have **unit weight** (`1`). If a node is unreachable, return `-1` for that node.

**Example:**  
Input: `adj = [[1,2],[0,2],[0,1,3],[2]]`, `src = 0`  
Output: `[0, 1, 1, 2]`

## Intuition & Approach
1. Since all edges have **unit weight**, the shortest path can be efficiently found using **Breadth-First Search (BFS)**.
2. BFS guarantees that we visit nodes in increasing order of distance from the source.
3. Steps:
    - Initialize `dist[src] = 0` and all other distances = `∞`.
    - Use a queue to perform BFS starting from the source.
    - For each node `u`, relax its neighbors `v` by updating `dist[v] = dist[u] + 1` if a shorter path is found.
    - Replace unreachable distances (∞) with `-1`.

## Code
```java
class Solution {
    // Function to find the shortest path from a source node to all other nodes
    public int[] shortestPath(ArrayList<ArrayList<Integer>> adj, int src) {
        int n = adj.size();
        int[] dist = new int[n];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;
        
        Queue<Integer> q = new LinkedList<>();
        q.add(src);
        
        while (!q.isEmpty()) {
            int u = q.poll();
            for (int v : adj.get(u)) {
                if (dist[u] + 1 < dist[v]) {
                    dist[v] = dist[u] + 1;
                    q.add(v);
                }
            }
        }
        
        for (int i = 0; i < n; i++) {
            if (dist[i] == Integer.MAX_VALUE) dist[i] = -1;
        }
        
        return dist;
    }
}
```
Time Complexity  
BFS visits each node and edge once: O(V + E)

Space Complexity  
Distance array: O(V)  
Queue: O(V) in worst case  
Overall: O(V + E)