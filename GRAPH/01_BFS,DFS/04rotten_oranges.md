## LeetCode 994 - Rotting Oranges

**Problem Description:**  
You are given a grid representing oranges where `0` = empty cell, `1` = fresh orange, and `2` = rotten orange. Every minute, fresh oranges adjacent to rotten ones (4-directionally) also become rotten. Return the minimum time needed for all oranges to rot, or `-1` if impossible.

**Intuition:**
- Treat rotten oranges as multiple sources for BFS.
- Push all initially rotten oranges into a queue with time = 0.
- Spread rot level by level, marking fresh oranges as visited when they rot.
- Track the maximum time encountered during BFS.
- After BFS, check if any fresh orange remained unvisited — if so, return `-1`.

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
    public int orangesRotting(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        int[][] vis = new int[m][n];
        Queue<Tuple> q = new LinkedList<>();

        // Push initially rotten oranges into queue
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == 2) {
                    q.add(new Tuple(i, j, 0));
                    vis[i][j] = 1;
                }
            }
        }

        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, 1, 0, -1};
        int ans = 0;

        // BFS to rot fresh oranges
        while(!q.isEmpty()) {
            Tuple t = q.remove();
            int row = t.first, col = t.second, time = t.third;
            ans = Math.max(ans, time);

            for(int i = 0; i < 4; i++) {
                int nrow = row + dr[i];
                int ncol = col + dc[i];
                if(nrow >= 0 && nrow < m && ncol >= 0 && ncol < n 
                   && vis[nrow][ncol] == 0 && grid[nrow][ncol] == 1) {
                    q.add(new Tuple(nrow, ncol, time + 1));
                    vis[nrow][ncol] = 1;
                }
            }
        }

        // Check if any fresh orange remained unrotted
        for(int i = 0; i < m; i++) {
            for(int j = 0; j < n; j++) {
                if(grid[i][j] == 1 && vis[i][j] == 0) return -1;
            }
        }
        return ans;
    }
}
```
## time comlexity
- O(4*m*n)= O(m*n)
## space comeplexity
- O(m*n)
vis array m*n , worst case m*n cells in queue
