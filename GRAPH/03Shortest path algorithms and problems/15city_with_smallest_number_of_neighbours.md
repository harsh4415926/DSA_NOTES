# LeetCode 1334 – Find the City With the Smallest Number of Neighbors at a Threshold Distance

## Problem
We are given `n` cities and weighted undirected roads.  
We must **find the city with the smallest number of reachable cities** such that the shortest path distance is **≤ distanceThreshold**.  
If multiple cities have the same count, **return the city with the largest index**.

---

## Approach – Floyd-Warshall (All-Pairs Shortest Paths)
Since we must compare distances between **all pairs of nodes**, Floyd-Warshall is a natural choice.

### Steps:
1. **Initialize distance matrix**:
    - Set `dist[i][j] = 1e9` (a large value) for no direct edge.
    - Set `dist[u][v] = w` and `dist[v][u] = w` for each road.
    - Set `dist[i][i] = 0`.

2. **Run Floyd-Warshall**:
    - For each intermediate node `k`, update all pairs `(i, j)`:
      ```
      if dist[i][k] != INF and dist[k][j] != INF:
          dist[i][j] = min(dist[i][j], dist[i][k] + dist[k][j]);
      ```
    - After this, `dist[i][j]` gives the shortest distance between every pair.

3. **Count reachable cities**:
    - For each city `i`, count how many `j` satisfy `dist[i][j] <= distanceThreshold`.

4. **Find the city with minimum count**:
    - If two cities have the same count, choose the **larger index**.

---

## Code

```java
class Solution {
    public int findTheCity(int n, int[][] edges, int distanceThreshold) {
        int[][] dist = new int[n][n];
        
        // Step 1: Initialize
        for(int i = 0; i < n; i++) {
            Arrays.fill(dist[i], (int)1e9);
            dist[i][i] = 0;
        }
        for(int[] e : edges) {
            int u = e[0], v = e[1], w = e[2];
            dist[u][v] = w;
            dist[v][u] = w;
        }

        // Step 2: Floyd-Warshall
        for(int k = 0; k < n; k++) {
            for(int i = 0; i < n; i++) {
                for(int j = 0; j < n; j++) {
                    if(dist[i][k] == 1e9 || dist[k][j] == 1e9) continue;
                    dist[i][j] = Math.min(dist[i][j], dist[i][k] + dist[k][j]);
                }
            }
        }

        // Step 3: Find city with min reachable count
        int city = -1;
        int minCount = Integer.MAX_VALUE;

        for(int i = 0; i < n; i++) {
            int count = 0;
            for(int j = 0; j < n; j++) {
                if(dist[i][j] <= distanceThreshold) {
                    count++;
                }
            }
            if(count <= minCount) { // <= ensures largest index in tie
                minCount = count;
                city = i;
            }
        }
        return city;
    }
}



Time Complexity: O(n³) – due to Floyd-Warshall triple nested loop.
Space Complexity: O(n²) – for the distance matrix.