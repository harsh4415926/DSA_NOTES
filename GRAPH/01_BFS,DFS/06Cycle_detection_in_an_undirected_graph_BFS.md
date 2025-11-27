## Cycle Detection in an Undirected Graph

**Problem Description:**  
Given an undirected graph represented as an edge list, determine if the graph contains any cycle.

**Intuition:**
- Use BFS with a parent tracker to detect cycles.
- For each unvisited node, perform BFS.
- If a visited neighbor is encountered that is **not the parent** of the current node, a cycle exists.
- Use a `vis[]` array to track visited nodes and a queue that stores `(node, parent)` pairs.

**Code:**
```java
class Pair {
    int first, second;
    Pair(int first, int second) {
        this.first = first;
        this.second = second;
    }
}

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
                if(helper(i, adj, vis)) return true;
            }
        }
        return false;
    }

    public boolean helper(int src, List<List<Integer>> adj, boolean[] vis) {
        Queue<Pair> q = new LinkedList<>();
        q.add(new Pair(src, -1));
        vis[src] = true;

        while(!q.isEmpty()) {
            Pair p = q.poll();
            int node = p.first, parent = p.second;

            for(int it : adj.get(node)) {
                if(!vis[it]) {
                    vis[it] = true;
                    q.add(new Pair(it, node));
                } else if(it != parent) {
                    return true; // cycle detected
                }
            }
        }
        return false;
    }
}
```
Time Complexity: O(V + E)   
Each vertex and edge is visited once.

Space Complexity: O(V + E)  
For adjacency list + queue + visited array.