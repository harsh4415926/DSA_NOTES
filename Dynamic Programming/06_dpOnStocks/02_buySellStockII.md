# Problem: Best Time to Buy and Sell Stock II
You're given an array prices where prices[i] is the stock's price on day i. You can complete as many transactions as you like (i.e., buy one and sell one share of the stock multiple times). However, you must sell the stock before you can buy again. Your goal is to find the maximum possible profit.

Example:

Input: prices = [7, 1, 5, 3, 6, 4]

Output: 7

Explanation: Buy on day 2 (price = 1) and sell on day 3 (price = 5), for a profit of 5 - 1 = 4. Then, buy on day 4 (price = 3) and sell on day 5 (price = 6), for a profit of 6 - 3 = 3. Total profit is 4 + 3 = 7.

---
## memoization
````java
class Solution {
    public int helper(int idx, int canBuy, int[] prices, int[][] dp) {
        // Base case: If we've run out of days, no more profit can be made.
        if (idx == prices.length) {
            return 0;
        }
        
        // Return the stored result if this state has already been computed.
        if (dp[idx][canBuy] != -1) return dp[idx][canBuy];

        // If we are allowed to buy today.
        if (canBuy == 1) {
            // Option 1: Buy today (-price) and move to a 'must sell' state (0).
            int buy = -prices[idx] + helper(idx + 1, 0, prices, dp);
            // Option 2: Don't buy today and stay in a 'can buy' state (1).
            int notBuy = helper(idx + 1, 1, prices, dp);
            // Store and return the maximum profit from the two options.
            return dp[idx][canBuy] = Math.max(buy, notBuy);
        }
        // If we are holding a stock and must sell.
        else {
            // Option 1: Sell today (+price) and move to a 'can buy' state (1).
            int sell = prices[idx] + helper(idx + 1, 1, prices, dp);
            // Option 2: Don't sell today and stay in a 'must sell' state (0).
            int notSell = helper(idx + 1, 0, prices, dp);
            // Store and return the maximum profit from the two options.
            return dp[idx][canBuy] = Math.max(sell, notSell);
        }
    }

    public int maxProfit(int[] prices) {
        int n = prices.length;
        // DP table stores results for each state [day][canBuy].
        int[][] dp = new int[n][2];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Start on day 0 with the ability to buy (state = 1).
        return helper(0, 1, prices, dp);
    }

    // Time Complexity: O(N)
    // - Where N is the number of days. There are N * 2 states,
    // - and each state is computed only once due to memoization.

    // Space Complexity: O(N)
    // - O(N * 2) for the DP table and O(N) for the recursion stack depth.
}
````
## tabulation
````java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        // dp[i][buy] -> max profit from day 'i' onwards.
        // buy = 1 means we can buy.
        // buy = 0 means we must sell.
        int[][] dp = new int[n + 1][2];
        
        // Base case: If there are no days left (i=n), profit is 0.
        dp[n][0] = 0;
        dp[n][1] = 0;
        
        // Iterate backwards from the second-to-last day.
        for (int i = n - 1; i >= 0; i--) {
            // Calculate max profit if we are allowed to buy on day 'i'.
            // Either we buy (-prices[i] + profit from next day in 'must sell' state)
            // Or we don't buy (profit from next day in 'can buy' state).
            dp[i][1] = Math.max((-prices[i] + dp[i + 1][0]), dp[i + 1][1]);
            
            // Calculate max profit if we must sell on day 'i'.
            // Either we sell (+prices[i] + profit from next day in 'can buy' state)
            // Or we don't sell (profit from next day in 'must sell' state).
            dp[i][0] = Math.max((prices[i] + dp[i + 1][1]), dp[i + 1][0]);
        }
        
        // The answer is the max profit starting from day 0, with the ability to buy.
        return dp[0][1];
    }

    // Time Complexity: O(N)
    // - Where N is the number of days. We iterate through the array once.

    // Space Complexity: O(N)
    // - For the N x 2 DP table.
}
````
## space optimization
````java
class Solution {
    public int maxProfit(int[] prices) {
        int n = prices.length;
        // [0] -> must sell profit, [1] -> can buy profit
        int[] ahead = new int[2];
        
        // Iterate backwards.
        for (int i = n - 1; i >= 0; i--) {
            int[] cur = new int[2];
            // Can buy: max profit from buying today vs. not buying today.
            cur[1] = Math.max((-prices[i] + ahead[0]), ahead[1]);
            // Must sell: max profit from selling today vs. not selling today.
            cur[0] = Math.max((prices[i] + ahead[1]), ahead[0]);
            // Update 'ahead' for the next iteration.
            ahead = cur;
        }
        
        // Final result for day 0 in the 'can buy' state.
        return ahead[1];
    }

    // Time Complexity: O(N)
    // - Single pass through the prices array.

    // Space Complexity: O(1)
    // - Uses constant extra space.
}
````



