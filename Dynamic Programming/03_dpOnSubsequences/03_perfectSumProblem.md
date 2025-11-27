## perfect sum prolem (gfg)
## Problem Statement
This code solves the "Perfect Sum Problem," which is about finding the count of all subsets in an array that add up to a specific target sum 🧮. This version correctly handles cases where the array contains zeros.
## memoization
````java
import java.util.Arrays;

class Solution {
    // Recursive helper to count subsets for 'target' using elements up to 'idx'.
    public int helper(int idx, int[] nums, int target, int[][] dp) {
        // Modulo for handling large numbers, as is often required in this problem.
        int mod = 1_000_000_007;

        // Base case for the first element (idx=0).
        if (idx == 0) {
            // If target is 0 and the element is 0, we have two subsets: {} and {0}.
            if (target == 0 && nums[0] == 0) return 2;
            // If target is 0 or the element equals the target, there's one subset.
            if (target == 0 || target == nums[0]) return 1;
            // Otherwise, it's not possible with this single element.
            return 0;
        }

        // Memoization: If this state is already computed, return the result.
        if (dp[idx][target] != -1) return dp[idx][target];

        // Option 1: Don't include the current element in the subset.
        int notTake = helper(idx - 1, nums, target, dp);

        // Option 2: Include the current element in the subset (if possible).
        int take = 0;
        if (target >= nums[idx]) {
            take = helper(idx - 1, nums, target - nums[idx], dp);
        }

        // Total ways = (ways by taking) + (ways by not taking).
        // Store the result in the dp table and return it.
        return dp[idx][target] = (take + notTake) % mod;
    }

    public int perfectSum(int[] nums, int target) {
        int n = nums.length;
        // dp[i][j] = # of subsets for sum 'j' using elements up to 'i'.
        int[][] dp = new int[n][target + 1];
        for (int[] row : dp) {
            Arrays.fill(row, -1);
        }

        // Start recursion from the last element and the full target sum.
        return helper(n - 1, nums, target, dp);
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N * target)
    // N is the number of elements. There are N*target states, and each is computed once.
    
    // Space Complexity: O(N * target)
    // O(N * target) for the DP table and O(N) for the recursion stack.
}
````
## tabulation
````java
import java.util.Arrays;

class Solution {
    public int perfectSum(int[] nums, int target) {
        int n = nums.length;
        // dp[i][j] = # of subsets with sum 'j' using elements up to index 'i'.
        int[][] dp = new int[n][target + 1];
        int mod = 1_000_000_007;

        // Base case for the first element (i=0).
        if (nums[0] == 0) {
            dp[0][0] = 2; // Two options for 0: pick it or not.
        } else {
            dp[0][0] = 1; // One option: don't pick the non-zero element.
        }
        
        // If the first element is not 0 and within the target bound,
        // there's one way to form a sum equal to its value.
        if (nums[0] != 0 && nums[0] <= target) {
            dp[0][nums[0]] = 1;
        }

        // Iterate through elements from the second one.
        for (int i = 1; i < n; i++) {
            // Iterate through all possible sums.
            for (int j = 0; j <= target; j++) {
                // Option 1: Don't include the current element.
                int notTake = dp[i - 1][j];
                
                // Option 2: Include the current element (if possible).
                int take = 0;
                if (j >= nums[i]) {
                    take = dp[i - 1][j - nums[i]];
                }
                
                // Total ways = (ways by taking) + (ways by not taking).
                // Apply modulo to prevent overflow.
                dp[i][j] = (take + notTake) % mod;
            }
        }
        
        // The final answer is the count for the target sum using all elements.
        return dp[n - 1][target];
    }

    // --- Complexity Analysis ---
    // Time Complexity: O(N * target)
    // We iterate through a 2D DP table of size N * target.
    
    // Space Complexity: O(N * target)
    // We use a 2D integer array to store the results.
}
````
