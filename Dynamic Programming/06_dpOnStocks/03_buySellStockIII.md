# Problem: Best Time to Buy and Sell Stock III
You are given an array prices where prices[i] is the price of a stock on day i. Find the maximum profit you can achieve from at most two transactions. You may not engage in multiple transactions simultaneously (i.e., you must sell the stock before you buy again).

Example:

Input: prices = [3, 3, 5, 0, 0, 3, 1, 4]

Output: 6

Explanation: Buy on day 4 (price = 0) and sell on day 6 (price = 3), profit = 3. Then buy on day 7 (price = 1) and sell on day 8 (price = 4), profit = 3. Total profit is 3 + 3 = 6.
## memoization
````java
class Solution {
    // helper(day, canBuy_flag, transactions_left, prices, dp)
    public int helper(int i, int canBuy, int remTrans, int[] prices, int[][][] dp) {
        // Base case: No more transactions or days left.
        if (remTrans == 0 || i == prices.length) return 0;
        
        // Return cached result.
        if (dp[i][canBuy][remTrans] != -1) return dp[i][canBuy][remTrans];

        // If we are allowed to buy.
        if (canBuy == 1) {
            // max(profit from buying today, profit from not buying today)
            int buy = -prices[i] + helper(i + 1, 0, remTrans, prices, dp);
            int notBuy = helper(i + 1, 1, remTrans, prices, dp);
            return dp[i][canBuy][remTrans] = Math.max(buy, notBuy);
        }
        // If we must sell.
        else {
            // max(profit from selling today, profit from not selling today)
            // A transaction is completed only after we sell.
            int sell = prices[i] + helper(i + 1, 1, remTrans - 1, prices, dp);
            int notSell = helper(i + 1, 0, remTrans, prices, dp);
            return dp[i][canBuy][remTrans] = Math.max(sell, notSell);
        }
    }

    public int maxProfit(int[] prices) {
        int n = prices.length;
        // DP state: [day][canBuy][transactions_left]
        int[][][] dp = new int[n][2][3];
        for (int i = 0; i < n; i++) {
            for (int j = 0; j <= 1; j++) {
                Arrays.fill(dp[i][j], -1);
            }
        }
        // Start on day 0, with the ability to buy, and 2 transactions remaining.
        return helper(0, 1, 2, prices, dp);
    }
    
    // Time Complexity: O(N * K)
    // - Where N is days and K is max transactions (here, K=2), so it's O(N).

    // Space Complexity: O(N * K)
    // - So, O(N) for the 3D DP table and recursion stack.
}
````
# tabulation
````java
class Solution {
public int maxProfit(int[] prices) {
int n = prices.length;
// DP state: dp[day][canBuy][transactions_left]
int[][][] dp = new int[n + 1][2][3];

        // Base cases: profit is 0 if no transactions are left, or no days are left.
        // These are automatically handled as 0 is the default int value in Java.
        for (int i = 0; i < n + 1; i++) {
            for (int j = 0; j <= 1; j++) {
                dp[i][j][0] = 0;
            }
        }
        for (int j = 0; j <= 1; j++) {
            for (int t = 0; t <= 2; t++) {
                dp[n][j][t] = 0;
            }
        }

        // Fill the table backwards from the last day.
        for (int i = n - 1; i >= 0; i--) {
            for (int j = 0; j <= 1; j++) { // j=1 (canBuy), j=0 (mustSell)
                for (int t = 1; t <= 2; t++) { // transactions left
                    if (j == 1) { // If we can buy
                        // max(buy today, don't buy today)
                        int buy = -prices[i] + dp[i + 1][0][t];
                        int notBuy = dp[i + 1][1][t];
                        dp[i][j][t] = Math.max(buy, notBuy);
                    } else { // If we must sell
                        // max(sell today, don't sell today)
                        // A transaction is completed only when we sell.
                        int sell = prices[i] + dp[i + 1][1][t - 1];
                        int notSell = dp[i + 1][0][t];
                        dp[i][j][t] = Math.max(sell, notSell);
                    }
                }
            }
        }
        
        // Result is the profit on day 0, with the ability to buy, and 2 transactions available.
        return dp[0][1][2];
    }

    // Time Complexity: O(N * 2 * K)
    // - Where N is days and K is max transactions (here, K=2), so it's O(N).

    // Space Complexity: O(N * 2 * K)
    // - So, O(N) for the 3D DP table.
}
````
## space optimised
````java
// class Solution {
//     public int helper(int i,int canBuy,int remTrans,int[] prices,int[][][] dp){
//         if(remTrans==0)return 0;
//         if(i==prices.length)return 0;
//         if(dp[i][canBuy][remTrans]!=-1)return dp[i][canBuy][remTrans];
//         if(canBuy==1){
//             int buy=-prices[i]+helper(i+1,0,remTrans,prices,dp);
//             int notBuy=helper(i+1,1,remTrans,prices,dp);
//             return dp[i][canBuy][remTrans]=Math.max(buy,notBuy);
//         }
//         else{
//             int sell=prices[i]+helper(i+1,1,remTrans-1,prices,dp);
//             int notSell=helper(i+1,0,remTrans,prices,dp);
//             return dp[i][canBuy][remTrans]=Math.max(sell,notSell);
//         }
//     }
//     public int maxProfit(int[] prices) {
//         int n=prices.length;
//         int[][][] dp=new int[n][2][3];
//         for(int i=0;i<n;i++){
//             for(int j=0;j<=1;j++){
//                 Arrays.fill(dp[i][j],-1);
//             }
//         }
//         return helper(0,1,2,prices,dp);
//     }
// }
class Solution {
    public int maxProfit(int[] prices) {
        int n=prices.length;
        int[][] after=new int[2][3];
        for(int j=0;j<=1;j++) {
            for(int t=0;t<=2;t++){
                after[j][t]=0;
            }
        }
        for(int i=n-1;i>=0;i--){
            int[][] cur=new int[2][3];
            for(int j=0;j<=1;j++){
                for(int t=1;t<=2;t++){
                     if(j==1){
            int buy=-prices[i]+after[0][t];
            int notBuy=after[1][t];
           cur[j][t]=Math.max(buy,notBuy);
        }
        else{
            int sell=prices[i]+after[1][t-1];
            int notSell=after[0][t];
             cur[j][t]=Math.max(sell,notSell);
        }
                }
            }
            after=cur;
        }
        return after[1][2];
    }
}
````


