# minimum falling path sum(leetcode)
in this both ends are variable 
## Problem Statement
This code solves the "Minimum Falling Path Sum" problem 💧. Given a square grid of integers, the goal is to find a "falling path" with the minimum possible sum. A falling path starts at any element in the first row and moves from a cell (row, col) to one of three cells in the next row: (row + 1, col - 1), (row + 1, col), or (row + 1, col + 1).
## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper to find the min falling path sum TO cell (i, j).
    public int helper(int i, int j, int[][] matrix, int[][] dp) {
        // Base case: If column is out of bounds, this path is invalid.
        // Return a large value so it's ignored by Math.min().
        if (j < 0 || j >= matrix.length) return Integer.MAX_VALUE;
        
        // Base case: If we're in the first row, the path sum is just the cell's value.
        if (i == 0) return matrix[i][j];
        
        // Memoization: If this state is already computed, return the result.
        if (dp[i][j] != -1) return dp[i][j];
        
        // Recursively find the min path sum from the three possible cells above.
        int upLeft = helper(i - 1, j - 1, matrix, dp);
        int up = helper(i - 1, j, matrix, dp);
        int upRight = helper(i - 1, j + 1, matrix, dp);
        
        // The min sum to (i,j) is its value + the minimum of the three paths from above.
        // Store the result in the dp table and return it.
        return dp[i][j] = matrix[i][j] + Math.min(upLeft, Math.min(up, upRight));
    }

    public int minFallingPathSum(int[][] matrix) {
        int n = matrix.length;
        int min = Integer.MAX_VALUE;
        
        // dp[i][j] stores the min falling path sum to reach cell (i, j).
        int[][] dp = new int[n][n];
        // Initialize dp table with -1 to mark states as uncomputed.
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        
        // A falling path can end in any column of the last row.
        // We need to calculate the path sum for each possible ending and find the minimum.
        for (int j = 0; j < n; j++) {
            min = Math.min(min, helper(n - 1, j, matrix, dp));
        }
        
        return min;
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N^2)
    // N is the side length of the square matrix. There are N*N states,
    // and we compute each one only once due to memoization.
    
    // Space Complexity: O(N^2)
    // O(N^2) for the DP table and O(N) for the recursion stack depth.
}
````
## tabulation
````java
import java.util.Arrays;

class Solution {
    public int minFallingPathSum(int[][] matrix) {
        int n = matrix.length;
        // dp[i][j] stores the min falling path sum to reach cell (i, j).
        int[][] dp = new int[n][n];

        // Base case: The min path to any cell in the first row is its own value.
        for (int j = 0; j < n; j++) {
            dp[0][j] = matrix[0][j];
        }

        // Iterate from the second row down to the bottom.
        for (int i = 1; i < n; i++) {
            for (int j = 0; j < n; j++) {
                // Get the min path sums from the three possible cells in the row above.
                // Handle edge cases (leftmost and rightmost columns) by using a large value.
                
                // Path from top-left.
                int upLeft = (j > 0) ? dp[i - 1][j - 1] : Integer.MAX_VALUE;
                // Path from directly above.
                int up = dp[i - 1][j];
                // Path from top-right.
                int upRight = (j < n - 1) ? dp[i - 1][j + 1] : Integer.MAX_VALUE;

                // Min sum for the current cell = its value + the minimum of the preceding paths.
                dp[i][j] = matrix[i][j] + Math.min(upLeft, Math.min(up, upRight));
            }
        }

        // The final answer is the minimum value in the last row of the dp table.
        int min = Integer.MAX_VALUE;
        for (int j = 0; j < n; j++) {
            min = Math.min(min, dp[n - 1][j]);
        }

        return min;
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N^2)
    // N is the side length of the matrix. We iterate through each of the N*N cells once.
    
    // Space Complexity: O(N^2)
    // We use an N*N DP table to store intermediate results.
}
````
