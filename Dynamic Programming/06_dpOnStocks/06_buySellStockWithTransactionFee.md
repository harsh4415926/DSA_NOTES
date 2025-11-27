# Problem: Best Time to Buy and Sell Stock with Transaction Fee
You're given an array prices for a stock and a transaction fee. You can complete as many transactions as you like, but you need to pay the transaction fee for each one. A transaction is defined as one buy and one sell. Find the maximum profit you can achieve.

Example:

Input: prices = [1, 3, 2, 8, 4, 9], fee = 2

Output: 8

Explanation:

Buy at prices[0] = 1 and sell at prices[3] = 8. Profit = (8 - 1) - 2 = 5.

Buy at prices[4] = 4 and sell at prices[5] = 9. Profit = (9 - 4) - 2 = 3.

Total profit = 5 + 3 = 8.

---
## memoization
````java
class Solution {
    // helper(day, canBuy_flag, prices, fee, dp)
    public int helper(int idx, int canBuy, int[] prices, int fee, int[][] dp) {
        // Base case: If we are past the last day, no more profit can be made.
        if (idx == prices.length) {
            return 0;
        }
        
        // Return the cached result if this state is already computed.
        if (dp[idx][canBuy] != -1) return dp[idx][canBuy];

        // If we are allowed to buy today.
        if (canBuy == 1) {
            // max(profit from buying today, profit from not buying today)
            int buy = -prices[idx] + helper(idx + 1, 0, prices, fee, dp);
            int notBuy = helper(idx + 1, 1, prices, fee, dp);
            return dp[idx][canBuy] = Math.max(buy, notBuy);
        }
        // If we are holding a stock and must sell.
        else {
            // max(profit from selling today, profit from not selling today)
            // After selling, subtract the transaction fee.
            int sell = prices[idx] + helper(idx + 1, 1, prices, fee, dp) - fee;
            int notSell = helper(idx + 1, 0, prices, fee, dp);
            return dp[idx][canBuy] = Math.max(sell, notSell);
        }
    }

    public int maxProfit(int[] prices, int fee) {
        int n = prices.length;
        // DP table stores results for each state [day][canBuy].
        int[][] dp = new int[n][2];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Start on day 0 with the ability to buy.
        return helper(0, 1, prices, fee, dp);
    }
    
    // Time Complexity: O(N)
    // - Where N is the number of days. There are N * 2 states,
    // - and each is computed once due to memoization.

    // Space Complexity: O(N)
    // - O(N * 2) for the DP table and O(N) for the recursion stack depth.
}
````

