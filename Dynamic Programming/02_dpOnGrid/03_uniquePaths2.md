# unique paths II (leetcode)
## Problem Statement
This code solves the "Unique Paths II" problem 🚧. It's a variation of the original problem where the grid now contains obstacles. The goal is to find the number of unique paths from the top-left to the bottom-right corner, moving only down and right, while avoiding the obstacles.
## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper to find paths to (i, j) while avoiding obstacles.
    public int helper(int i, int j, int[][] dp, int[][] grid) {
        // Base case: If we go out of bounds, it's an invalid path.
        if (i < 0 || j < 0) return 0;
        
        // If the current cell is an obstacle, there are 0 paths through it.
        if (grid[i][j] == 1) return 0;
        
        // Base case: If we reach the start (and it's not an obstacle), we found one valid path.
        if (i == 0 && j == 0) return 1;
        
       
        
        // Memoization: If we've already computed paths for this cell, return the result.
        if (dp[i][j] != -1) return dp[i][j];
        
        // Paths from the cell above.
        int up = helper(i - 1, j, dp, grid);
        // Paths from the cell to the left.
        int left = helper(i, j - 1, dp, grid);
        
        // Store and return the total paths for the current cell.
        return dp[i][j] = left + up;
    }

    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        
        // dp[i][j] stores the number of unique paths to cell (i, j).
        int[][] dp = new int[m][n];
        // Initialize dp table with -1 to mark states as uncomputed.
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        
        // Start the recursion from the destination cell.
        return helper(m - 1, n - 1, dp, obstacleGrid);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(m * n)
    // We compute the result for each of the m*n cells only once.
    
    // Space Complexity: O(m * n)
    // O(m * n) for the DP table and O(m + n) for the recursion stack.
}
````
## tablulation
````java
import java.util.Arrays;

class Solution {
    public int uniquePathsWithObstacles(int[][] obstacleGrid) {
        int m = obstacleGrid.length;
        int n = obstacleGrid[0].length;
        
        // If the starting cell itself is an obstacle, no path is possible.
        if (obstacleGrid[0][0] == 1) return 0;
        
        // dp[i][j] will store the number of paths to reach cell (i, j).
        int[][] dp = new int[m][n];
        
        // Base case: There's one way to reach the starting cell.
        dp[0][0] = 1;
        
        // --- Fix Note ---
        // The original loop logic was flawed. It has been corrected below to a
        // standard tabulation approach for clarity and correctness.
        
        // Iterate through each cell of the grid.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // If the current cell is an obstacle, no paths can pass through it.
                if (obstacleGrid[i][j] == 1) {
                    dp[i][j] = 0;
                    continue; // Skip to the next cell.
                }

                // If a valid cell above exists, add its paths.
                if (i > 0) {
                    dp[i][j] += dp[i - 1][j];
                }
                // If a valid cell to the left exists, add its paths.
                if (j > 0) {
                    dp[i][j] += dp[i][j - 1];
                }
            }
        }
        
        // The result is the number of paths to the bottom-right destination cell.
        return dp[m - 1][n - 1];
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(m * n)
    // We iterate through every cell of the grid exactly once.
    
    // Space Complexity: O(m * n)
    // We use an auxiliary 2D array of size m*n to store the results.
}
````

