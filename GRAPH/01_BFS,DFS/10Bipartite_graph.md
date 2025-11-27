## Bipartite Graph Check (DFS)

**Problem Description:**  
- Given a graph in adjacency list format, determine if it is **bipartite**.  
A graph is bipartite if its vertices can be divided into two sets such that no two vertices in the same set are adjacent.
- or you can say We want to check if it is possible to color the graph using 2 colors such that no two adjacent nodes share the same color.

**Intuition:**
- Use DFS to try coloring the graph with **two colors (0 and 1)**.
- If a node is uncolored, assign it a color and recursively color all neighbors with the opposite color.
- If a neighbor already has the same color as the current node, the graph is **not bipartite**.

**Code:**
```java
class Solution {
    public boolean isBipartite(int[][] graph) {
        int n = graph.length;
        int[] color = new int[n];
        Arrays.fill(color, -1); // -1 means uncolored
        
        for(int i = 0; i < n; i++) {
            if(color[i] == -1) {
                if(!dfs(i, graph, color, 0)) return false;
            }
        }
        return true;
    }

    public boolean dfs(int node, int[][] graph, int[] color, int col) {
        color[node] = col;
        for(int nbr : graph[node]) {
            if(color[nbr] == -1) {
                if(!dfs(nbr, graph, color, 1 - col)) return false;
            } else if(color[nbr] == color[node]) {
                return false; // same color neighbor found
            }
        }
        return true;
    }
}
```
Time Complexity: O(V + E)  
Each vertex and edge is visited once during DFS.

Space Complexity: O(V)  
For the color array and recursion stack