# Problem: Critical Connections in a Network (LeetCode 1192)

## Problem Description
You are given:
- An integer `n` (number of servers, labeled `0` to `n-1`).
- A list of `connections` (each an undirected edge between two servers).

A connection is **critical** if removing it disconnects the graph.  
Return all such critical connections.

**Example:**  
Input: `n = 4`, `connections = [[0,1],[1,2],[2,0],[1,3]]`  
Output: `[[1,3]]`

---

## Intuition & Approach
This is the classic **bridge-finding problem** in an undirected graph, solved using **Tarjan’s Algorithm** with DFS timestamps:
- `tin[u]`: entering time of node `u`.
- `low[u]`: lowest entering time reachable from node `u` via back edges.

### Key Idea:
- Perform DFS.
- For each edge `(u → v)`:
    - If `v` is unvisited, recursively DFS on `v`  and then update `low[u] = min(low[u], low[v])`, if(low[v]>tin[u])means without the edge between u and v , v can not reach to a node less than or equal to u  so it is a bridge
    - If `v` is visited and not the parent, update `low[u] = min(low[u], tin[v])`.,it cant be a bridge since it has been earlier been visited so there must be a different path between them other than the edge 
- After visiting all children:
    - If `low[v] > tin[u]`, then `(u,v)` is a **bridge (critical connection)**.

### Steps:
1. Build adjacency list.
2. Initialize arrays: `vis`, `tin`, `low`.
3. Run DFS starting from node `0` (graph is guaranteed to be connected in this problem).
4. Collect edges where `low[v] > tin[u]`.

---

## Code
```java
class Solution {
    private int timer = 1;

    private void dfs(int node, int parent, List<List<Integer>> adj,
                     int[] vis, int[] tin, int[] low, List<List<Integer>> ans) {
        vis[node] = 1;
        tin[node] = low[node] = timer++;
        
        for (int nbr : adj.get(node)) {
            if (nbr == parent) continue;
            if (vis[nbr] == 0) {
                dfs(nbr, node, adj, vis, tin, low, ans);
                low[node] = Math.min(low[node], low[nbr]);
                
                // Condition for bridge
                if (low[nbr] > tin[node]) {
                    List<Integer> edge = new ArrayList<>();
                    edge.add(node);
                    edge.add(nbr);
                    ans.add(edge);
                }
            } else {
                // Back edge
                low[node] = Math.min(low[node], tin[nbr]);
            }
        }
    }

    public List<List<Integer>> criticalConnections(int n, List<List<Integer>> connections) {
        List<List<Integer>> adj = new ArrayList<>();
        for (int i = 0; i < n; i++) adj.add(new ArrayList<>());
        
        for (List<Integer> it : connections) {
            int u = it.get(0), v = it.get(1);
            adj.get(u).add(v);
            adj.get(v).add(u);
        }
        
        int[] vis = new int[n];
        int[] tin = new int[n];
        int[] low = new int[n];
        List<List<Integer>> ans = new ArrayList<>();
        
        dfs(0, -1, adj, vis, tin, low, ans);
        return ans;
    }
}
```
Time Complexity  
Building adjacency list: O(n + e)  
Single DFS traversal: O(n + e)  
Overall: O(n + e)

Space Complexity  
Adjacency list: O(n + e)  
Arrays vis, tin, low: O(n)  
Recursion stack: O(n)  
Overall: O(n + e)
