# Problem: Coin Change II (LeetCode 518)
You're given an array of coin denominations coins and a total amount. Your task is to find the number of distinct combinations of coins that can sum up to that amount. You can assume you have an infinite supply of each type of coin.

Example:

Input: amount = 5, coins = [1, 2, 5]

Output: 4

Explanation: There are four ways to make up the amount:

5

2 + 2 + 1

2 + 1 + 1 + 1

1 + 1 + 1 + 1 + 1
## memoization
````java
class Solution {
    // Recursive helper to count the combinations.
    public int helper(int idx, int amount, int[] coins, int[][] dp) {
        // Base case: We are at the first coin (index 0).
        if (idx == 0) {
            // An amount of 0 can always be made in one way (by choosing no coins).
            if (amount == 0) return 1;
            // If the remaining amount is perfectly divisible by the coin, there's one way.
            if (amount % coins[idx] == 0) return 1;
            // Otherwise, it's impossible with just this coin.
            return 0;
        }
        
        // If this subproblem (state) has been solved, return the stored result.
        if (dp[idx][amount] != -1) return dp[idx][amount];

        // Choice 1: Don't take the current coin.
        // The number of ways is what we can get from the remaining coins.
        int notTake = helper(idx - 1, amount, coins, dp);
        
        // Choice 2: Take the current coin.
        int take = 0;
        if (amount >= coins[idx]) {
            // Number of ways for the remaining amount, staying at the same coin
            // because we can use it multiple times.
            take = helper(idx, amount - coins[idx], coins, dp);
        }
        
        // Total combinations = (ways if we take the coin) + (ways if we don't).
        return dp[idx][amount] = take + notTake;
    }

    public int change(int amount, int[] coins) {
        int n = coins.length;
        // dp[i][j] stores the number of ways to make amount 'j' using coins up to index 'i'.
        int[][] dp = new int[n][amount + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Start the recursion from the last coin.
        return helper(n - 1, amount, coins, dp);
    }
}
````


