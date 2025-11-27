## LeetCode 802 – Find Eventual Safe States

**Problem Description:**  
A node in a directed graph is *safe* if every possible path starting from it leads to a terminal node (a node with no outgoing edges).  
Return all safe nodes in sorted order.

**Intuition:**
- Perform DFS to detect **nodes that are part of or lead to cycles (unsafe)**.
- Use three arrays:
    - `vis[node]`: visited nodes.
    - `pathVis[node]`: nodes in the current DFS path (used to detect cycles).
    - `mark[node]`: `1` for safe nodes, `2` for unsafe nodes.
- If a node leads to a cycle, mark it unsafe (`mark[node] = 2`); otherwise mark it safe (`mark[node] = 1`).
- After DFS, collect all nodes marked as safe.

**Code:**
```java
class Solution {
    public boolean dfs(int node, int[][] graph, int[] vis, int[] pathVis, int[] mark) {
        vis[node] = 1;
        pathVis[node] = 1;

        for(int nbr : graph[node]) {
            if(vis[nbr] == 0) {
                if(dfs(nbr, graph, vis, pathVis, mark)) {
                    mark[node] = 2; // unsafe
                    return true;
                }
            } else if(pathVis[nbr] == 1) {
                mark[node] = 2; // cycle detected → unsafe
                return true;
            }
        }

        pathVis[node] = 0;
        mark[node] = 1; // safe node
        return false;
    }

    public List<Integer> eventualSafeNodes(int[][] graph) {
        int V = graph.length;
        int[] vis = new int[V];
        int[] pathVis = new int[V];
        int[] mark = new int[V]; // 1 → safe, 2 → unsafe

        for(int i = 0; i < V; i++) {
            if(vis[i] == 0) {
                dfs(i, graph, vis, pathVis, mark);
            }
        }

        List<Integer> ans = new ArrayList<>();
        for(int i = 0; i < V; i++) {
            if(mark[i] == 1) ans.add(i);
        }
        return ans;
    }
}
```
Time Complexity: O(V + E)  
Each node and edge is processed once.

Space Complexity: O(V)  
For vis, pathVis, mark, and recursion stack.