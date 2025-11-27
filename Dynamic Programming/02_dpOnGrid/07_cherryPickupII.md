# cherry pickup II(leetcode)

## Problem Statement
This code solves the "Cherry Pickup II" problem 🍒. Two robots start at the top corners of a grid ((0, 0) and (0, n-1)). They both move down one row at a time and can move to the left, center, or right column in the next row. If both robots land on the same cell, they only collect the cherries once. The goal is to find the maximum total number of cherries the two robots can collect by the time they both reach the bottom row.
## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper for the state: (row i, robot1 at col j1, robot2 at col j2).
    public int helper(int i, int j1, int j2, int[][] grid, int[][][] dp) {
        int m = grid.length;
        int n = grid[0].length;
        
        // Base case: If a robot is out of the grid's horizontal bounds, this path is invalid.
        // Return a large negative number so it's ignored by Math.max().
        if (j1 < 0 || j1 >= n || j2 < 0 || j2 >= n) return (int)(-1e9);
        
        // Base case: If at the last row, collect the cherries and stop.
        if (i == m - 1) {
            if (j1 == j2) return grid[i][j1]; // Collect once if on the same cell.
            else return grid[i][j1] + grid[i][j2]; // Collect from both if on different cells.
        }
        
        // Memoization: If this state is already computed, return the result.
        if (dp[i][j1][j2] != -1) return dp[i][j1][j2];
        
        int max = (int)(-1e9);
        // Explore all 9 possible combined moves for the two robots.
        // 'a' is the column change for robot 1, 'b' is for robot 2.
        for (int a = -1; a <= 1; a++) {
            for (int b = -1; b <= 1; b++) {
                int currentCherries;
                // Cherries collected at the current row 'i'.
                if (j1 == j2) currentCherries = grid[i][j1];
                else currentCherries = grid[i][j1] + grid[i][j2];
                
                // Max cherries = current collection + max from the best future path.
                max = Math.max(max, currentCherries + helper(i + 1, j1 + a, j2 + b, grid, dp));
            }
        }
        
        // Store the result for the current state and return it.
        return dp[i][j1][j2] = max;
    }

    public int cherryPickup(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        
        // dp[i][j1][j2] stores max cherries from row 'i' down, with robots at j1 and j2.
        int[][][] dp = new int[m][n][n];
        for (int i = 0; i < m; i++) {
            for (int j = 0; j < n; j++) {
                Arrays.fill(dp[i][j], -1);
            }
        }
        
        // Start recursion: row 0, robot1 at col 0, robot2 at col n-1.
        return helper(0, 0, n - 1, grid, dp);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(M * N^2)
    // M is rows, N is columns. There are M*N*N states, and each is computed once.
    
    // Space Complexity: O(M * N^2)
    // O(M * N^2) for the DP table and O(M) for the recursion stack.
}
````
## tabulation
````java
import java.util.Arrays;

class Solution {
    public int cherryPickup(int[][] grid) {
        int m = grid.length;
        int n = grid[0].length;
        // dp[i][j1][j2] = max cherries from row 'i' to the bottom,
        // with robot1 at column j1 and robot2 at column j2.
        int[][][] dp = new int[m][n][n];

        // Base Case: Fill the last row of the dp table.
        // The max cherries from the last row is just what's collected there.
        for (int j1 = 0; j1 < n; j1++) {
            for (int j2 = 0; j2 < n; j2++) {
                if (j1 == j2)
                    dp[m - 1][j1][j2] = grid[m - 1][j1]; // Collect once.
                else
                    dp[m - 1][j1][j2] = grid[m - 1][j1] + grid[m - 1][j2]; // Collect from both.
            }
        }

        // Iterate from the second-to-last row upwards to the top.
        for (int i = m - 2; i >= 0; i--) {
            // For every possible position of robot1 (j1) and robot2 (j2).
            for (int j1 = 0; j1 < n; j1++) {
                for (int j2 = 0; j2 < n; j2++) {
                    // Find the max score the robots can get from the NEXT row (i+1).
                    int maxFutureScore = Integer.MIN_VALUE;
                    // Check all 9 possible combined moves.
                    for (int a = -1; a <= 1; a++) {
                        for (int b = -1; b <= 1; b++) {
                            // Ensure the next move is within the grid's bounds.
                            if (j1 + a >= 0 && j1 + a < n && j2 + b >= 0 && j2 + b < n) {
                                maxFutureScore = Math.max(maxFutureScore, dp[i + 1][j1 + a][j2 + b]);
                            }
                        }
                    }

                    // Cherries collected at the CURRENT row 'i'.
                    int currentCherries;
                    if (j1 == j2) currentCherries = grid[i][j1];
                    else currentCherries = grid[i][j1] + grid[i][j2];

                    // Total score for this state = current cherries + max future score.
                    dp[i][j1][j2] = currentCherries + maxFutureScore;
                }
            }
        }

        // The final answer is the value at the starting state: row 0, robots at 0 and n-1.
        return dp[0][0][n - 1];
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(M * N^2)
    // We iterate through M*N*N states, and the inner work is a constant 9 steps.
    
    // Space Complexity: O(M * N^2)
    // We use a 3D DP table of size M*N*N.
}
````