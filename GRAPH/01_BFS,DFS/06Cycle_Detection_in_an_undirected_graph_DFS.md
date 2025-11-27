## Cycle Detection in an Undirected Graph (DFS)

**Problem Description:**  
Given an undirected graph represented as an edge list, determine if the graph contains any cycle using DFS.

**Intuition:**
- Use DFS to explore each vertex.
- Track the parent of each node during DFS.
- If a visited neighbor is found that is **not the parent** of the current node, a cycle exists.
- Recursively visit all unvisited neighbors to detect cycles in all connected components.

**Code:**
```java
class Solution {
    public boolean isCycle(int V, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        for(int i = 0; i < V; i++) {
            adj.add(new ArrayList<Integer>());
        }
        for(int i = 0; i < edges.length; i++) {
            adj.get(edges[i][0]).add(edges[i][1]);
            adj.get(edges[i][1]).add(edges[i][0]);
        }

        boolean[] vis = new boolean[V];
        for(int i = 0; i < V; i++) {
            if(!vis[i]) {
                if(dfs(i, adj, vis, -1)) return true;
            }
        }
        return false;
    }

    public boolean dfs(int src, List<List<Integer>> adj, boolean[] vis, int parent) {
        vis[src] = true;
        for(int it : adj.get(src)) {
            if(!vis[it]) {
                if(dfs(it, adj, vis, src)) return true;
            } else if(it != parent) {
                return true; // cycle detected
            }
        }
        return false;
    }
}
```
Time Complexity: O(V + E)  
Each vertex and edge is visited once.

Space Complexity: O(V + E)  
For adjacency list + visited array + recursion stack.