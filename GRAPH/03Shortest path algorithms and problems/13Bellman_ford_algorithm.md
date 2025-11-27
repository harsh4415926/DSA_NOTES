# Problem: Bellman-Ford Algorithm for Shortest Path with Negative Weights

## Intuition
The **Bellman-Ford algorithm** is used to find the shortest path from a source to all vertices when:
- The graph may contain **negative edge weights**.
- **Dijkstra's algorithm cannot be applied** (since it fails on negative weights).

### Key Idea:
1. Initialize distances from the source as infinity (or a large value), except for the source which is `0`.
2. **Relax all edges `V-1` times** (where `V` is the number of vertices).
    - Relaxation means: if `dist[u] + weight < dist[v]`, then update `dist[v]`.
    - After `V-1` iterations, all shortest paths will have been found (because the longest simple path has at most `V-1` edges).
3. Perform **one more iteration** to check for **negative weight cycles**:
    - If any edge can still be relaxed, then the graph contains a negative weight cycle.


### Important:
- **Bellman-Ford naturally works on directed graphs.**
- If you need to use it on an **undirected graph**, you must **add both directions for every edge** (i.e., write the edge in both directions manually).

## Code
```java
class Solution {
    public int[] bellmanFord(int V, int[][] edges, int src) {
        int[] dist = new int[V];
        Arrays.fill(dist, (int)1e8);
        dist[src] = 0;
        
        // V-1 iterations: relax all edges
        for(int i = 0; i < V - 1; i++) {
            for(int[] it : edges) {
                int u = it[0];
                int v = it[1];
                int w = it[2];
                if(dist[u] != 1e8 && dist[u] + w < dist[v]) {
                    dist[v] = dist[u] + w;
                }
            }
        }
        
        // Check for negative weight cycle (Nth iteration)
        for(int[] it : edges) {
            int u = it[0];
            int v = it[1];
            int w = it[2];
            if(dist[u] != 1e8 && dist[u] + w < dist[v]) {
                return new int[]{-1}; // Negative cycle detected
            }
        }
        
        return dist;
    }
}
```

### Time Complexity: O(V × E)
                    For V-1 iterations, we relax E edges each time.

### Space Complexity: O(V)
                  For the distance array.