# Problem: Coin Change (LeetCode 322)
You're given an array of coin denominations coins and a total amount. Your goal is to find the fewest number of coins to make up that amount. If the amount can't be made with the given coins, return -1. You have an infinite supply of each coin.

Example:

Input: coins = [1, 2, 5], amount = 11

Output: 3

Explanation: The amount 11 can be formed by 5 + 5 + 1, which uses 3 coins.
## memoization
````java
class Solution {
    // Recursive helper to find the minimum coins.
    public int helper(int idx, int[] coins, int amount, int[][] dp) {
        // Base case: If we're only considering the first coin (at index 0).
        if (idx == 0) {
            // If the amount is perfectly divisible by the coin, return the count.
            if (amount % coins[idx] == 0) return amount / coins[idx];
            // Otherwise, it's impossible with just this coin.
            return (int) 1e9; // Use a large value for "infinity".
        }
        
        // If this subproblem is already solved, return the result.
        if (dp[idx][amount] != -1) return dp[idx][amount];

        // Choice 1: Don't take the current coin.
        int notTake = helper(idx - 1, coins, amount, dp);
        
        // Choice 2: Take the current coin.
        int take = (int) 1e9;
        if (coins[idx] <= amount) {
            // It's 1 (for this coin) + result for the remaining amount.
            // Note: We stay at 'idx' because we can reuse the same coin.
            take = 1 + helper(idx, coins, amount - coins[idx], dp);
        }
        
        // Store and return the minimum of the two choices.
        return dp[idx][amount] = Math.min(notTake, take);
    }

    public int coinChange(int[] coins, int amount) {
        int n = coins.length;
        // dp[i][j] stores min coins for amount 'j' using coins up to index 'i'.
        int[][] dp = new int[n][amount + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);

        // Start the recursion from the last coin.
        int ans = helper(n - 1, coins, amount, dp);

        // If 'ans' is our "infinity" value, the amount is impossible to make.
        if (ans >= (int) 1e9) return -1;
        
        return ans;
    }
}
````
## tabulation
````java
class Solution {
    public int coinChange(int[] coins, int amount) {
        int n=coins.length;
           int[][] dp=new int[n][amount+1];
           for(int j=0;j<=amount;j++){
            if(j%coins[0]==0)dp[0][j]=j/coins[0];
            else dp[0][j]=(int)1e9;
           }
           for(int i=1;i<n;i++){
            for(int j=0;j<=amount;j++){
                int notTake=dp[i-1][j];
                int take=(int)1e9;
        if(coins[i]<=j)take=1+dp[i][j-coins[i]];
        dp[i][j]=Math.min(notTake,take);
            }
           }
           if(dp[n-1][amount]>=1e9)return -1;
           else return dp[n-1][amount];
    }
}
````
