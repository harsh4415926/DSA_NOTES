## GFG – Number of Distinct Islands

**Problem Description:**  
Given a 2D grid of `0`s and `1`s, count the number of **distinct islands**.  
Two islands are considered distinct if their shapes are different (translation allowed, rotation or reflection not considered).

**Intuition:**
- Perform DFS from every unvisited land cell (`1`).
- Record the **relative positions** of all cells in the island with respect to the starting cell `(row0, col0)`.
- Store the shape as a list of relative coordinates in a set to avoid duplicates.
- The size of the set gives the number of distinct islands.

**Code:**
```java
class Solution {
    void dfs(int row, int col, int[][] grid, int[] dr, int[] dc, int[][] vis, List<String> l, int row0, int col0) {
        int n = grid.length, m = grid[0].length;
        vis[row][col] = 1;
        
        // Store relative position to starting cell
        l.add((row - row0) + " " + (col - col0));
        
        for(int i = 0; i < 4; i++) {
            int nrow = row + dr[i];
            int ncol = col + dc[i];
            if(nrow >= 0 && nrow < n && ncol >= 0 && ncol < m 
               && vis[nrow][ncol] == 0 && grid[nrow][ncol] == 1) {
                dfs(nrow, ncol, grid, dr, dc, vis, l, row0, col0);
            }
        }
    }
  
    int countDistinctIslands(int[][] grid) {
        int n = grid.length, m = grid[0].length;
        int[][] vis = new int[n][m];
        int[] dr = {-1, 0, 1, 0};
        int[] dc = {0, 1, 0, -1};
        HashSet<List<String>> st = new HashSet<>();
        
        for(int i = 0; i < n; i++) {
            for(int j = 0; j < m; j++) {
                if(vis[i][j] == 0 && grid[i][j] == 1) {
                    List<String> l = new ArrayList<>();
                    dfs(i, j, grid, dr, dc, vis, l, i, j);
                    st.add(l);
                }
            }
        }
        return st.size();
    }
}
```
Time Complexity: O(n * m)  
Each cell is visited once, DFS explores 4 directions.

Space Complexity: O(n * m)  
For visited array + recursion stack + storage of island shapes in set.