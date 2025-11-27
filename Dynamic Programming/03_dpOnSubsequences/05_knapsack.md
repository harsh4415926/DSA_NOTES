# Problem: 0/1 Knapsack(gfg)
You are given N items, where each item has a specific value and a weight. You are also given a knapsack with a maximum weight capacity of W. Your task is to select a subset of these items to put into the knapsack such that the total value is maximized, without the total weight of the selected items exceeding the capacity W. You can either pick an item completely or not at all.
## memoization
````java
class Solution {
    public static int helper(int idx, int[] wt, int W, int[] val, int[][] dp) {

        // Base Case: If we're at the first item (index 0).
        if (idx == 0) {
            // If we have enough capacity, we can take it, otherwise value is 0.
            if (W >= wt[idx]) return val[idx];
            return 0;
        }
        
        // If this state (idx, W) is already computed, return the stored result.
        if (dp[idx][W] != -1) return dp[idx][W];
        
        // --- Explore the two choices for the current item ---

        // Choice 1: Take the current item.
        int take = Integer.MIN_VALUE; // Initialize with a very small value.
        if (W >= wt[idx]) { // Only possible if we have enough capacity.
            // Value is current item's value + max value from remaining items with reduced capacity.
            take = val[idx] + helper(idx - 1, wt, W - wt[idx], val, dp);
        }
        
        // Choice 2: Do not take the current item.
        // Value is the max value from remaining items with the same capacity.
        int notTake = helper(idx - 1, wt, W, val, dp);
        
        // Store the maximum of the two choices in the DP table and return it.
        return dp[idx][W] = Math.max(take, notTake);
    }

    static int knapsack(int W, int val[], int wt[]) {
        int n = val.length;
        // Create a DP table to store the results of subproblems.
        int[][] dp = new int[n][W + 1];
        // Initialize the DP table with -1 to mark states as "not yet computed".
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        // Start the recursive calculation from the last item (n-1).
        return helper(n - 1, wt, W, val, dp);
    }
}
````
## tabulation
````java
class Solution {
    static int knapsack(int W, int val[], int wt[]) {
        int n = val.length;
        // dp[i][j] = max value for first 'i' items with capacity 'j'.
        int[][] dp = new int[n][W + 1];

        // Base Case: Fill the row for the first item (index 0).
        for (int j = 0; j <= W; j++) {
            if (j >= wt[0]) {
                // If we can fit the first item, take its value.
                dp[0][j] = val[0];
            }
        }

        // Fill the rest of the table for items 1 to n-1.
        for (int i = 1; i < n; i++) {
            for (int j = 0; j <= W; j++) {

                // Don't take item 'i': value is from the row above.
                int notTake = dp[i - 1][j];

                // Take item 'i':
                int take = Integer.MIN_VALUE;
                if (j >= wt[i]) {
                    // Its value + best value from remaining capacity in row above.
                    take = val[i] + dp[i - 1][j - wt[i]];
                }

                // Store the better of the two choices.
                dp[i][j] = Math.max(take, notTake);
            }
        }

        // Final answer is in the bottom-right cell.
        return dp[n - 1][W];
    }
}
````