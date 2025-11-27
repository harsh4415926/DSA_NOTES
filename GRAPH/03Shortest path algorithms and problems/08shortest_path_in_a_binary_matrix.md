# Shortest Path in Binary Matrix (LeetCode 1091)
## Problem Description
Given an `n x n` binary matrix `grid`, return the length of the **shortest clear path** from the top-left cell `(0,0)` to the bottom-right cell `(n-1,n-1)`.  
A clear path contains only `0`s and allows movement in **8 directions** (horizontal, vertical, diagonal).  
If no such path exists, return `-1`.
## Thought Process
- Since all moves have equal weight (distance = 1), **BFS is sufficient** to find the shortest path.
- **Dijkstra would also work**, but it's unnecessary because it introduces an extra `O(log n)` factor with no added benefit for unit weights.
- We simply perform BFS, store `(row, col, level)` in a queue, and **return `level` as soon as we reach the destination cell** `(m-1, n-1)`.
- We maintain a `visited` matrix to avoid revisiting cells.

---

## Code using bfs
```java
class Triplet {
    int first;
    int second;
    int third;
    Triplet(int first, int second, int third) {
        this.first = first;
        this.second = second;
        this.third = third;
    }
}

class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid[0][0] != 0) return -1;
        int m = grid.length;
        int n = grid[0].length;
        int[] dr = {-1, -1, 0, +1, +1, +1, 0, -1};
        int[] dc = {0, +1, +1, +1, 0, -1, -1, -1};
        
        Queue<Triplet> q = new LinkedList<>();
        q.add(new Triplet(0, 0, 1));
        int[][] vis = new int[m][n];
        
        while (q.size() > 0) {
            Triplet t = q.poll();
            int row = t.first;
            int col = t.second;
            int level = t.third;
            
            if (row == m - 1 && col == n - 1) return level;
            
            for (int i = 0; i < 8; i++) {
                int nrow = row + dr[i];
                int ncol = col + dc[i];
                if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n &&
                    grid[nrow][ncol] == 0 && vis[nrow][ncol] == 0) {
                    q.add(new Triplet(nrow, ncol, level + 1));
                    vis[nrow][ncol] = 1;
                }
            }
        }
        return -1;
    }
}
```
## code using Dijsktra
```java
class Triplet {
    int first;
    int second;
    int third;

    Triplet(int first, int second, int third) {
        this.first = first;
        this.second = second;
        this.third = third;
    }
}

class Solution {
    public int shortestPathBinaryMatrix(int[][] grid) {
        if (grid[0][0] != 0) return -1;
        int m = grid.length;
        int n = grid[0].length;

        PriorityQueue<Triplet> q = new PriorityQueue<>((a, b) -> a.first - b.first);
        int[][] dist = new int[m][n];
        for (int i = 0; i < dist.length; i++) {
            Arrays.fill(dist[i], Integer.MAX_VALUE);
        }

        q.add(new Triplet(0, 0, 0));
        dist[0][0] = 0;

        int[] dr = {-1, -1, 0, +1, +1, +1, 0, -1};
        int[] dc = {0, +1, +1, +1, 0, -1, -1, -1};

        while (!q.isEmpty()) {
            Triplet t = q.poll();
            int distance = t.first;
            int row = t.second;
            int col = t.third;

            for (int i = 0; i < 8; i++) {
                int nrow = row + dr[i];
                int ncol = col + dc[i];

                if (nrow >= 0 && nrow < m && ncol >= 0 && ncol < n 
                    && grid[nrow][ncol] == 0 
                    && distance + 1 < dist[nrow][ncol]) {
                        
                    dist[nrow][ncol] = distance + 1;
                    q.add(new Triplet(distance + 1, nrow, ncol));
                }
            }
        }

        if (dist[m-1][n-1] == Integer.MAX_VALUE) return -1;
        else return dist[m-1][n-1] + 1;
    }
}
```



