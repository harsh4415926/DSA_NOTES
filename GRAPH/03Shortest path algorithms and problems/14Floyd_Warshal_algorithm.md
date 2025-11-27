# Problem: Floyd-Warshall Algorithm for All-Pairs Shortest Path

## Intuition
The **Floyd-Warshall algorithm** is used to find the shortest distances between **all pairs of vertices** in a weighted graph.  
It works even when there are **negative edge weights** (as long as there is no negative weight cycle).

### Key Idea:
1. Initialize a `dist[][]` matrix such that:
    - `dist[i][j]` = weight of edge (i → j) if it exists, else a large value (e.g., 1e8 for infinity).
    - `dist[i][i] = 0` for all vertices.
2. For each intermediate vertex `k`, try to **relax every pair (i, j)** via `k`:
    - If going through `k` improves the path `i → j`, update it:  
      `dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j])`
3. After the triple loop, `dist[i][j]` contains the shortest distance between every pair.
4. **Detecting negative cycles:**
    - If `dist[i][i] < 0` for any `i`, the graph contains a negative weight cycle.

### Important:
- **Floyd-Warshall works on directed graphs naturally.**
- For undirected graphs, add edges in **both directions**.
- It is simpler to implement but has **higher time complexity (O(V³))**, so it is used for **small graphs** or when all-pairs shortest paths are required.
- can be used to detect negative cycles just like bellman ford 
- - **If no negative edges:** You can use **Dijkstra from each node (O(V·(V+E)logV))**, which is usually **faster than Floyd-Warshall (O(V³))**
## Code
```java
class Solution {
    public void floydWarshall(int[][] dist) {
        int V = dist.length;
        
        for(int k = 0; k < V; k++) {
            for(int i = 0; i < V; i++) {
                for(int j = 0; j < V; j++) {
                    // Update distance i → j via k if it's shorter
                    if(dist[i][k] != 1e8 && dist[k][j] != 1e8) {
                        dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
                    }
                }
            }
        }
        
        // If detecting negative cycles is required:
        // for(int i = 0; i < V; i++) {
        //     if(dist[i][i] < 0) {
        //         // Negative cycle detected
        //     }
        // }
    }
}

```
Time Complexity: O(V³)  
Space Complexity: O(1) extra (in-place), or O(V²) if separate copy
