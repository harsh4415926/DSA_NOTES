## Cycle Detection in a Directed Graph (DFS)--- GFG

**Problem Description:**  
Given a directed graph represented as an edge list, determine if the graph contains a **cycle**.

**Intuition:**
- Use DFS with a **recursion stack tracker (`pathVis`)** to detect cycles.
- `vis[node]` keeps track of all visited nodes.
- `pathVis[node]` keeps track of nodes in the **current DFS path**.
- If a neighbor is already in the current DFS path (`pathVis[nbr] == 1`), a cycle exists.
- After exploring all neighbors, remove the node from the current path (`pathVis[node] = 0`).

**Code:**
```java
class Solution {
    public boolean dfs(int node, List<List<Integer>> adj, int[] vis, int[] pathVis) {
        vis[node] = 1;
        pathVis[node] = 1;
        
        for(int nbr : adj.get(node)) {
            if(vis[nbr] == 0) {
                if(dfs(nbr, adj, vis, pathVis)) return true;
            } else if(pathVis[nbr] == 1) {
                return true; // cycle detected
            }
        }
        pathVis[node] = 0;
        return false;
    }

    public boolean isCyclic(int V, int[][] edges) {
        List<List<Integer>> adj = new ArrayList<>();
        for(int i = 0; i < V; i++) adj.add(new ArrayList<>());

        for(int i = 0; i < edges.length; i++) {
            int u = edges[i][0];
            int v = edges[i][1];
            adj.get(u).add(v);
        }

        int[] vis = new int[V];
        int[] pathVis = new int[V];

        for(int i = 0; i < V; i++) {
            if(vis[i] == 0) {
                if(dfs(i, adj, vis, pathVis)) return true;
            }
        }
        return false;
    }
}
```
Time Complexity: O(V + E)  
Each vertex and edge is visited once.

Space Complexity: O(V)  
For vis, pathVis, and recursion stack.
