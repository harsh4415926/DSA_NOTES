# Problem: Knapsack with Duplicate Items (Unbounded Knapsack)
You're given a set of N items, each with a specific value and weight, and a knapsack with a maximum capacity W. Your task is to fill the knapsack with items to maximize the total value. The key rule is that you can use any item any number of times.

Example:

Input: W = 8, val[] = {1, 4, 5, 7}, wt[] = {1, 3, 4, 5}

Output: 11

Explanation: You can achieve the maximum value by picking the item with weight 3 (value 4) and the item with weight 5 (value 7). The total weight is 3 + 5 = 8, and the total value is 4 + 7 = 11.
## memoization
````java
class Solution {
    public static int helper(int idx, int capacity, int[] val, int[] wt, int[][] dp) {
        // Base case: We are at the last item (index 0).
        if (idx == 0) {
            if (capacity == 0) return 0; // If no capacity left, value is 0.
            // Check how many times we can take this last item.
            if (capacity / wt[idx] > 0) {
                int x = capacity / wt[idx]; // Number of times the item fits.
                return x * val[idx];        // Total value from taking it 'x' times.
            }
            return 0; // Cannot take the last item.
        }

        // If this subproblem is already solved, return the stored result.
        if (dp[idx][capacity] != -1) return dp[idx][capacity];

        // Choice 1: Take the current item.
        int take = 0;
        if (capacity >= wt[idx]) {
            // Value is current item's value + max value from the remaining capacity.
            // Note: We stay at 'idx' because we can use this item again.
            take = val[idx] + helper(idx, capacity - wt[idx], val, wt, dp);
        }
        
        // Choice 2: Don't take the current item.
        // Move on to the next item with the same capacity.
        int notTake = helper(idx - 1, capacity, val, wt, dp);
        
        // Store and return the maximum value from the two choices.
        return dp[idx][capacity] = Math.max(take, notTake);
    }

    static int knapSack(int val[], int wt[], int capacity) {
        int n = val.length;
        // dp[i][j] stores max value for capacity 'j' using items up to index 'i'.
        int[][] dp = new int[n][capacity + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);

        // Start the recursion from the last item.
        return helper(n - 1, capacity, val, wt, dp);
    }
}
````

