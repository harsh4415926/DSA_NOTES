# unique paths(leetcode)
## Problem Statement
This code solves the "Unique Paths" problem 🤖. Given an m x n grid, the goal is to find the number of unique paths a robot can take to get from the top-left corner (0, 0) to the bottom-right corner (m-1, n-1). The robot can only move down or right at any point.

## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper to find unique paths to cell (i, j).
    public int helper(int i, int j, int[][] dp) {
        // Base case: If we're at the start (0,0), there's exactly one path.
        if (i == 0 && j == 0) return 1;

        // Base case: If we go out of bounds, it's not a valid path.
        if (i < 0 || j < 0) return 0;

        // Memoization: If this state is already computed, return the result.
        if (dp[i][j] != -1) return dp[i][j];

        // Paths coming from the cell above.
        int up = helper(i - 1, j, dp);
        // Paths coming from the cell to the left.
        int left = helper(i, j - 1, dp);

        // Total paths to (i,j) is the sum from up and left.
        // Store the result in the dp table before returning.
        return dp[i][j] = left + up;
    }

    public int uniquePaths(int m, int n) {
        // dp[i][j] stores the number of unique paths to cell (i, j).
        int[][] dp = new int[m][n];
        // Initialize dp table with -1 to mark states as uncomputed.
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        // Start the recursion from the destination cell (m-1, n-1).
        return helper(m - 1, n - 1, dp);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(m * n)
    // There are m*n states, and each is computed only once due to memoization.

    // Space Complexity: O(m * n)
    // O(m * n) for the DP table and O(m + n) for the recursion stack.
}
````
## tabulation
````java
import java.util.Arrays;

class Solution {
    public int uniquePaths(int m, int n) {
        // dp[i][j] will store the number of unique paths to reach cell (i, j).
        int[][] dp = new int[m][n];
        
        // Base case: There is exactly one way to be at the starting cell (0, 0).
        dp[0][0] = 1;
        
        // Iterate through each cell in the grid.
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                // Skip the starting cell as we've already initialized it.
                if (i == 0 && j == 0) continue;
                
                // Paths coming from the cell above.
                int up = 0;
                // Paths coming from the cell to the left.
                int left = 0;
                
                // If a cell above exists, get its path count.
                if (i - 1 >= 0) up = dp[i - 1][j];
                // If a cell to the left exists, get its path count.
                if (j - 1 >= 0) left = dp[i][j - 1];
                
                // Total paths to (i, j) is the sum of paths from up and left.
                dp[i][j] = up + left;
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
