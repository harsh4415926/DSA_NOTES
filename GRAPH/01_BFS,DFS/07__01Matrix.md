## LeetCode 542 – 01 Matrix

**Problem Description:**  
Given an `m x n` binary matrix `mat`, return the distance of the nearest `0` for each cell.  
The distance between two adjacent cells is 1.

**Intuition:**
- Perform a **multi-source BFS**:
    - Push all `0` cells into the queue as starting points with distance `0`.
    - Mark them visited and set their distance as `0`.
- Process the queue:
    - For each popped cell, explore its 4 neighbors.
    - If neighbor is unvisited and `1`, set its distance = current distance + 1 and push to queue.
- BFS ensures that each cell gets the shortest distance to a `0`.

**Code:**
```java
class Tuple {
    int first, second, third;
    Tuple(int first, int second, int third) {
        this.first = first;
        this.second = second;
        this.third = third;
    }
}

class Solution {
    public int[][] updateMatrix(int[][] mat) {
        int m = mat.length, n = mat[0].length;
        int[][] vis = new int[m][n];
        int[][] ans = new int[m][n];
        Queue<Tuple> q = new ArrayDeque<>();

        // Push all 0's into the queue
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(mat[i][j] == 0) {
                    q.add(new Tuple(i, j, 0));
                    vis[i][j] = 1;
                    ans[i][j] = 0;
                }
            }
        }

        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, 1, 0, -1};

        // BFS traversal
        while(!q.isEmpty()) {
            Tuple t = q.poll();
            int row = t.first, col = t.second, dist = t.third;

            for(int i = 0; i < 4; i++) {
                int nrow = row + dr[i];
                int ncol = col + dc[i];

                if(nrow >= 0 && nrow < m && ncol >= 0 && ncol < n 
                   && vis[nrow][ncol] == 0 && mat[nrow][ncol] == 1) {
                    ans[nrow][ncol] = dist + 1;
                    vis[nrow][ncol] = 1;
                    q.add(new Tuple(nrow, ncol, dist + 1));
                }
            }
        }
        return ans;
    }
}
```
Time Complexity: O(m * n)  
Each cell is visited at most once in BFS.

Space Complexity: O(m * n)  
For visited matrix, result matrix, and BFS queue.