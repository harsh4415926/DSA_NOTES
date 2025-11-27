# Shortest Path in Directed Acyclic Graph (DAG) using Topological Sort

## Problem Description
Given a **Directed Acyclic Graph (DAG)** with `V` vertices and `E` edges (each edge has a weight), find the shortest path from a source vertex (0 in this problem) to all other vertices.  
If a vertex is unreachable from the source, return `-1` for that vertex.

**Example:**  
Input: `V = 6, edges = [[0,1,2],[0,4,1],[1,2,3],[4,2,2],[4,5,4],[2,3,6],[5,3,1]]`  
Output: `[0, 2, 3, 6, 1, 5]`

## Intuition & Approach
1. Since the graph is a **DAG**, we can use **topological sorting** to efficiently compute shortest paths.
2. Steps:
    - Perform DFS to generate a topological order of nodes.
    - Initialize distances from source: `dist[src] = 0`, others = `∞`.
    - Process nodes in topological order, relaxing all outgoing edges.
    - Replace unreachable vertices (∞) with `-1`.

**Key Points:**
- Topological sort ensures we always process nodes after all predecessors, making single pass relaxation possible.
- Time complexity is linear relative to edges and vertices (`O(V+E)`).
  Here’s a **complete, precise short note** for future reference about shortest paths in a DAG using topological sort:

---
1. **Topological Sort Property**:

    * In a topological sort, for any edge `u → v`, `u` appears **before** `v`.
    * Therefore, **all nodes reachable from a node `u` appear to the right** of `u` in the topo order.

2. **Processing for Shortest Path from Source**:

    * Initialize distances from source: `dist[src] = 0`, all others `INF`.
    * Process nodes **in topological order** from top of the stack.
    * **Skip nodes until you reach the source** — their distance is `INF`, so they cannot be relaxed.
    * Once the source is reached, **relax all outgoing edges**.

3. **Why this works**:

    * All nodes reachable from the source are to its right → they will be relaxed properly.
    * Nodes to the left of the source are **guaranteed unreachable** → safe to ignore.

4. **Implementation Tip**:

    * Always check `if(dist[u] != INF)` before relaxing edges to avoid processing unreachable nodes.


## Code
```java
class Pair {
    int first;
    int second;
    Pair(int first, int second) {
        this.first = first;
        this.second = second;
    }
}

class Solution {
    public void dfs(int node, List<List<Pair>> adj, int[] vis, Stack<Integer> st) {
        vis[node] = 1;
        for (Pair it : adj.get(node)) {
            int nbr = it.first;
            if (vis[nbr] == 0) dfs(nbr, adj, vis, st);
        }
        st.push(node);
    }

    public int[] shortestPath(int V, int E, int[][] edges) {
        int src = 0; // Source vertex
        List<List<Pair>> adj = new ArrayList<>();
        for (int i = 0; i < V; i++) adj.add(new ArrayList<>());
        
        for (int i = 0; i < E; i++) {
            int a = edges[i][0];
            int b = edges[i][1];
            int c = edges[i][2];
            adj.get(a).add(new Pair(b, c));
        }
        
        int[] vis = new int[V];
        Stack<Integer> st = new Stack<>();
        for (int i = 0; i < V; i++) {
            if (vis[i] == 0) dfs(i, adj, vis, st);
        }
        
        int[] dist = new int[V];
        Arrays.fill(dist, Integer.MAX_VALUE);
        dist[src] = 0;
        
        while (!st.isEmpty()) {
            int node = st.pop();
            if (dist[node] == Integer.MAX_VALUE) continue;
            for (Pair p : adj.get(node)) {
                int nbr = p.first;
                int edgeDist = p.second;
                if (dist[node] + edgeDist < dist[nbr]) {
                    dist[nbr] = dist[node] + edgeDist;
                }
            }
        }
        
        for (int i = 0; i < V; i++) {
            if (dist[i] == Integer.MAX_VALUE) dist[i] = -1;
        }
        
        return dist;
    }
}
```
Time Complexity  
Topological Sort (DFS): O(V + E)  
Relaxing edges in topological order: O(E)  
Overall: O(V + E)

Space Complexity  
Adjacency list: O(V + E)  
Visited array + stack + distance array: O(V)  
Overall: O(V + E)
