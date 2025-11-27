## LeetCode 130 – Surrounded Regions

**Problem Description:**  
Given an `m x n` board containing `'X'` and `'O'`, capture all regions surrounded by `'X'`.  
Any `'O'` that is not connected to the border should be flipped to `'X'`.

**Intuition:**
- `'O'` cells connected to the border **cannot be captured**, so mark them as safe using DFS or BFS.
- Perform DFS from all border `'O'` cells to mark them as visited (safe).
- After traversal, any `'O'` that is unvisited is surrounded and must be flipped to `'X'`.

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
    public void solve(char[][] board) {
        int m = board.length, n = board[0].length;
        boolean[][] vis = new boolean[m][n];
        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, 1, 0, -1};

        // DFS from border rows
        for(int j = 0; j < n; j++) {
            if(board[0][j] == 'O' && !vis[0][j])
                dfs(0, j, board, vis, dr, dc);
            if(board[m-1][j] == 'O' && !vis[m-1][j])
                dfs(m-1, j, board, vis, dr, dc);
        }

        // DFS from border columns
        for(int i = 0; i < m; i++) {
            if(board[i][0] == 'O' && !vis[i][0])
                dfs(i, 0, board, vis, dr, dc);
            if(board[i][n-1] == 'O' && !vis[i][n-1])
                dfs(i, n-1, board, vis, dr, dc);
        }

        // Flip unvisited 'O' to 'X'
        for(int a = 0; a < m; a++) {
            for(int b = 0; b < n; b++) {
                if(board[a][b] == 'O' && !vis[a][b]) {
                    board[a][b] = 'X';
                }
            }
        }
    }

    public void dfs(int i, int j, char[][] board, boolean[][] vis, int[] dr, int[] dc) {
        vis[i][j] = true;

        for(int t = 0; t < 4; t++) {
            int nrow = i + dr[t];
            int ncol = j + dc[t];
            if(nrow >= 0 && nrow < board.length && ncol >= 0 && ncol < board[0].length 
               && board[nrow][ncol] == 'O' && !vis[nrow][ncol]) {
                dfs(nrow, ncol, board, vis, dr, dc);
            }
        }
    }
}
```
Time Complexity: O(m * n)  
Each cell is visited at most once.

Space Complexity: O(m * n)  
For visited array and recursion stack space.