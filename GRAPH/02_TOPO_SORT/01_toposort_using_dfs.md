## Topological Sort (DFS-based)

**Problem Description:**  
Given a directed graph with `V` vertices and `E` edges, return a topological ordering of the graph.  
A *topological order* is a linear ordering of vertices such that for every directed edge `u → v`, vertex `u` comes before `v` in the ordering.  
*(This is only possible for Directed Acyclic Graphs – DAGs.)*

**Intuition:**
- Use DFS to explore nodes.
- After fully visiting a node (all its neighbors processed), push it onto a stack.
- After DFS completes for all nodes, pop nodes from the stack to get the topological order.

**Code:**
```java
class Solution {
    public static ArrayList<Integer> topoSort(int V, int[][] edges) {
        // Build adjacency list
        List<List<Integer>> adj = new ArrayList<>();
        for(int i = 0; i < V; i++) {
            adj.add(new ArrayList<>());
        }
        for(int i = 0; i < edges.length; i++) {
            adj.get(edges[i][0]).add(edges[i][1]);
        }

        int[] vis = new int[V];
        Stack<Integer> st = new Stack<>();

        // Perform DFS on all unvisited nodes
        for(int i = 0; i < V; i++) {
            if(vis[i] == 0) {
                dfs(i, vis, adj, st);
            }
        }

        // Pop from stack to get topological order
        ArrayList<Integer> ans = new ArrayList<>();
        while(!st.isEmpty()) {
            ans.add(st.pop());
        }
        return ans;
    }

    public static void dfs(int node, int[] vis, List<List<Integer>> adj, Stack<Integer> st) {
        vis[node] = 1;
        for(int nbr : adj.get(node)) {
            if(vis[nbr] == 0) {
                dfs(nbr, vis, adj, st);
            }
        }
        st.push(node); // add after visiting all neighbors
    }
}
```
Time Complexity: O(V + E)  
Each node and edge is processed once.

Space Complexity: O(V)  
For recursion stack, visited array, and stack to store result.