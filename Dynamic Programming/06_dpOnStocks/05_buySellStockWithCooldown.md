# Problem: Best Time to Buy and Sell Stock with Cooldown
You're given an array prices for a stock. You can complete as many transactions as you like, with one constraint: after you sell your stock, you cannot buy stock on the next day (i.e., there is a 1-day cooldown period). Find the maximum profit.

Example:

Input: prices = [1, 2, 3, 0, 2]

Output: 3

Explanation: Transactions: [buy, sell, cooldown, buy, sell]. Profit = (2 - 1) + (2 - 0) = 3.

## memoization
````java
class Solution {
    // helper(day, canBuy_flag, prices, dp)
    public int helper(int idx, int canBuy, int[] prices, int[][] dp) {
        // Base case: If we are past the last day, no profit can be made.
        if (idx >= prices.length) {
            return 0;
        }
        
        // Return the cached result if this state is already computed.
        if (dp[idx][canBuy] != -1) return dp[idx][canBuy];

        // If we are allowed to buy today.
        if (canBuy == 1) {
            // max(profit from buying today, profit from not buying today)
            int buy = -prices[idx] + helper(idx + 1, 0, prices, dp);
            int notBuy = helper(idx + 1, 1, prices, dp);
            return dp[idx][canBuy] = Math.max(buy, notBuy);
        }
        // If we are holding a stock and must sell.
        else {
            // max(profit from selling today, profit from not selling today)
            // After selling, we skip one day (idx+2) for the cooldown.
            int sell = prices[idx] + helper(idx + 2, 1, prices, dp);
            int notSell = helper(idx + 1, 0, prices, dp);
            return dp[idx][canBuy] = Math.max(sell, notSell);
        }
    }

    public int maxProfit(int[] prices) {
        int n = prices.length;
        // DP table stores results for each state [day][canBuy].
        int[][] dp = new int[n][2];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Start on day 0 with the ability to buy.
        return helper(0, 1, prices, dp);
    }
    
    // Time Complexity: O(N)
    // - Where N is the number of days. There are N * 2 states,
    // - and each is computed once due to memoization.

    // Space Complexity: O(N)
    // - O(N * 2) for the DP table and O(N) for the recursion stack depth.
}
````


