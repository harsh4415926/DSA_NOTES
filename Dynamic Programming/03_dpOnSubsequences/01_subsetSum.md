# subset sum(gfg)
## Problem Statement
This code solves the "Subset Sum" problem 🎯. Given an array of non-negative integers and a target sum, the goal is to determine if there exists a subset of the given array whose elements add up to the target sum.

## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper: can we make 'target' using elements up to 'idx'?
    static Boolean helper(int idx, int target, int[] arr, int[][] dp) {
        // Base case: Target sum of 0 is always possible (by picking an empty subset).
        if (target == 0) return true;
        
        // Base case: If we're at the first element, a solution is possible
        // only if that element's value is equal to the target.
        if (idx == 0) return (arr[0] == target);
        
        // Memoization: If this state is already computed, return the result.
        if (dp[idx][target] != -1) return dp[idx][target] == 1;
        
        // --- Note: The logic below has been refactored for clarity ---

        // Option 1: Don't include the current element in the subset.
        // Check if the target can be met with the remaining elements.
        boolean notTake = helper(idx - 1, target, arr, dp);
        
        // Option 2: Include the current element in the subset (if possible).
        boolean take = false;
        if (target >= arr[idx]) {
            take = helper(idx - 1, target - arr[idx], arr, dp);
        }
        
        // A solution exists if either the 'take' or 'notTake' path is successful.
        boolean result = take || notTake;
        
        // Store the result in the dp table (1 for true, 0 for false) and return.
        dp[idx][target] = result ? 1 : 0;
        return result;
    }

    static Boolean isSubsetSum(int arr[], int sum) {
        int n = arr.length;
        // dp[i][j] will store if a sum 'j' is possible using elements up to index 'i'.
        // Values: 1 for true, 0 for false, -1 for uncomputed.
        int[][] dp = new int[n][sum + 1];
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }
        
        // Start recursion from the last element and the full target sum.
        return helper(n - 1, sum, arr, dp);
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(N * sum)
    // N is the number of elements. There are N*sum states, and each is computed once.
    
    // Space Complexity: O(N * sum)
    // O(N * sum) for the DP table and O(N) for the recursion stack.
}
````
## tabulation
````java
import java.util.Arrays;

class Solution {
    static Boolean isSubsetSum(int arr[], int sum) {
        int n = arr.length;
        // dp[i][j] = true if a sum 'j' is possible using elements up to index 'i'.
        boolean[][] dp = new boolean[n][sum + 1];

        // Base case: A sum of 0 is always possible with an empty subset.
        for (int i = 0; i < n; i++) {
            dp[i][0] = true;
        }

        // Base case: With only the first element, we can form a sum equal to its value.
        if (arr[0] <= sum) {
            dp[0][arr[0]] = true;
        }

        // Iterate through the array elements, starting from the second one.
        for (int i = 1; i < n; i++) {
            // Iterate through all possible target sums.
            for (int j = 1; j <= sum; j++) {
                // Option 1: Don't include the current element arr[i].
                // The result is the same as for the previous elements.
                boolean notTake = dp[i - 1][j];
                
                // Option 2: Include the current element arr[i] (if possible).
                boolean take = false;
                if (j >= arr[i]) {
                    take = dp[i - 1][j - arr[i]];
                }
                
                // The current state is true if either the 'take' or 'notTake' option works.
                dp[i][j] = take || notTake;
            }
        }
        
        // The final answer is in the bottom-right corner of the table.
        return dp[n - 1][sum];
    }
    
    // --- Complexity Analysis ---
    // Time Complexity: O(N * sum)
    // We iterate through a 2D DP table of size N * sum.
    
    // Space Complexity: O(N * sum)
    // We use a 2D boolean array to store the results.
}
````