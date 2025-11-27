## LeetCode 733 - Flood Fill

**Problem Description:**  
Given a 2D image represented as a grid, a starting pixel `(sr, sc)`, and a new color, replace the color of the starting pixel and all connected pixels (4-directionally) of the same original color with the new color. Return the modified image.

**Intuition:**
- Use DFS to explore all 4-directionally connected pixels with the same original color.
- Replace the color of the current pixel with the new color.
- Recursively visit valid neighboring pixels that match the original color and are not already changed to the new color.

**Code:**
```java
class Solution {
    public int[][] floodFill(int[][] image, int sr, int sc, int color) {
        int[] dr = {-1, 0, +1, 0};
        int[] dc = {0, +1, 0, -1};
        int m = image.length;
        int n = image[0].length;
        int initial = image[sr][sc];
        
        dfs(sr, sc, image, initial, color, dr, dc);
        return image;
    }
    
    public void dfs(int sr, int sc, int[][] image, int initial, int neww, int[] dr, int[] dc) {
        int m = image.length;
        int n = image[0].length;
        image[sr][sc] = neww;
        
        for(int i = 0; i < 4; i++) {
            int nrow = sr + dr[i];
            int ncol = sc + dc[i];
            if(nrow >= 0 && nrow < m && ncol >= 0 && ncol < n 
               && image[nrow][ncol] == initial && image[nrow][ncol] != neww)
                dfs(nrow, ncol, image, initial, neww, dr, dc);
        }
    }
}
```
## time complexity
- O(m * n)
## space complexity
- O(m * n)