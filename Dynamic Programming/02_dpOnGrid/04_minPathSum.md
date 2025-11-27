# minimum Path sum (leetcode)
## Problem Statement
This code solves the "Minimum Path Sum" problem 💰. Given a grid filled with non-negative numbers, the goal is to find a path from the top-left corner to the bottom-right corner that minimizes the sum of all numbers along its path. You can only move down or right.
## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper to find the minimum path sum to cell (i, j).
    public int helper(int i, int j, int[][] grid, int[][] dp) {
        // Base case: An invalid path. Return a very large value to ensure it's never chosen.
        if (i < 0 || j < 0) return Integer.MAX_VALUE;
        
        // Base case: If at the start (0,0), the sum is just the cell's value.
        if (i == 0 && j == 0) return grid[i][j];
        
        
        
        // Memoization: If we've already computed the sum for this cell, return it.
        if (dp[i][j] != -1) return dp[i][j];
        
        // Min path sum from the cell above.
        int up = helper(i - 1, j, grid, dp);
        // Min path sum from the cell to the left.
        int left = helper(i, j - 1, grid, dp);
        
        // The min sum to the current cell is its value plus the minimum of the two prior paths.
        // Store the result in the dp table before returning.
        return dp[i][j] = (grid[i][j] + Math.min(up, left));
    }

    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        // dp[i][j] stores the minimum path sum to reach cell (i, j).
        int[][] dp = new int[m][n];
        // Initialize dp table with -1 to mark states as uncomputed.
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        
        // Start the recursion from the destination cell (m-1, n-1).
        return helper(m - 1, n - 1, grid, dp);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(m * n)
    // There are m*n states, and each is computed only once due to memoization.
    
    // Space Complexity: O(m * n)
    // O(m * n) for the DP table and O(m + n) for the recursion stack depth.
}
````
## tabulation
````java
import java.util.Arrays;

class Solution {
    public int minPathSum(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        // dp[i][j] will store the minimum path sum to reach cell (i, j).
        int[][] dp = new int[m][n];
        
        // Iterate through each cell of the grid to fill the dp table.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // Base case: For the starting cell, the min sum is its own value.
                if (i == 0 && j == 0) {
                    dp[i][j] = grid[i][j];
                    continue;
                }
                
                // Initialize paths from up and left to a large value.
                int up = Integer.MAX_VALUE;
                int left = Integer.MAX_VALUE;
                
                // If a cell above exists, get its minimum path sum.
                if (i > 0) up = dp[i - 1][j];
                // If a cell to the left exists, get its minimum path sum.
                if (j > 0) left = dp[i][j - 1];
                
                // Min sum for the current cell is its value + the minimum of the preceding paths.
                dp[i][j] = grid[i][j] + Math.min(up, left);
            }
        }
        
        // The result is the minimum path sum stored in the destination cell.
        return dp[m - 1][n - 1];
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(m * n)
    // We iterate through every cell of the grid exactly once.
    
    // Space Complexity: O(m * n)
    // We use an auxiliary 2D array of size m*n.
}
````