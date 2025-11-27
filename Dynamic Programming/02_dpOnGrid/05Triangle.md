# Triangle (leetcode)
before this , problems were with both ends fixed but in this one reacing point is variable
## Problem Statement
This code solves the "Triangle" problem (LeetCode 120) 🔺. You're given a triangle-shaped array of numbers. The goal is to find the minimum path sum from the top of the triangle to the bottom. At each step, you can move from a number to one of the two numbers directly below it in the next row.

## memoization
````java
import java.util.*;

class Solution {
    // Recursive helper to find the min path sum from (i, j) to the bottom.
    public int helper(int i, int j, List<List<Integer>> triangle, int[][] dp) {
        // Base case: If in the last row, the path sum is just the element's value.
        if (i == triangle.size() - 1) {
            return triangle.get(i).get(j);
        }

        // Memoization: If this state is already computed, return the result.
        if (dp[i][j] != -1) return dp[i][j];

        // Calculate the min path sum from the cell directly below.
        int down = helper(i + 1, j, triangle, dp);
        // Calculate the min path sum from the cell diagonally down-right.
        int downRight = helper(i + 1, j + 1, triangle, dp);

        // Min path from (i,j) = current value + the minimum of the two downward paths.
        // Store the result in the dp table and return it.
        return dp[i][j] = (triangle.get(i).get(j) + Math.min(down, downRight));
    }

    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();

        // dp[i][j] stores the min path sum starting from cell (i, j).
        int[][] dp = new int[n][n];
        // Initialize dp table with -1 to mark states as uncomputed.
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        
        // Start the recursion from the top of the triangle (0, 0).
        return helper(0, 0, triangle, dp);
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(N^2)
    // N is the number of rows. There are N*(N+1)/2 states, and we compute each once.
    
    // Space Complexity: O(N^2)
    // O(N^2) for the DP table and O(N) for the recursion stack depth.
}
````
## tabulation
````java
import java.util.*;

class Solution {
    public int minimumTotal(List<List<Integer>> triangle) {
        int n = triangle.size();
        
        // dp[i][j] will store the min path sum from cell (i, j) to the bottom.
        int[][] dp = new int[n][n];
        
        // Base case: The min path from the last row is just the elements themselves.
        for (int j = 0; j < n; j++) {
            dp[n - 1][j] = triangle.get(n - 1).get(j);
        }
        
        // Iterate from the second-to-last row upwards to the top.
        for (int i = n - 2; i >= 0; i--) {
            // Iterate through each element in the current row.
            for (int j = i; j >= 0; j--) {
                // Get the pre-computed min paths from the two possible next steps.
                int down = dp[i + 1][j];
                int downRight = dp[i + 1][j + 1];
                
                // Calculate the min path for the current cell.
                dp[i][j] = triangle.get(i).get(j) + Math.min(down, downRight);
            }
        }
        
        // The final answer is the min path sum starting from the top of the triangle.
        return dp[0][0];
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N^2)
    // N is the number of rows. The nested loops iterate through each cell of the triangle once.
    
    // Space Complexity: O(N^2)
    // We use an n*n DP table to store the results.
}
````