# Problem: Best Time to Buy and Sell Stock IV
You are given an integer k and an array of stock prices prices. Find the maximum profit you can achieve with at most k transactions. You must sell the stock before you can buy again.

Example:

Input: k = 2, prices = [3, 2, 6, 5, 0, 3]

Output: 7

Explanation: Buy on day 2 (price = 2) and sell on day 3 (price = 6), profit = 4. Then buy on day 5 (price = 0) and sell on day 6 (price = 3), profit = 3. Total profit is 4 + 3 = 7.

---
## memoization

````java
class Solution {
// helper(day, transaction_number, prices, k, dp)
public int helper(int i, int trans, int[] prices, int k, int[][] dp) {
// Base case: All k transactions are done, or no days are left.
if (trans == k * 2) return 0;
if (i == prices.length) return 0;

        // Return cached result.
        if (dp[i][trans] != -1) return dp[i][trans];

        // If transaction number is even, it's a "buy" turn.
        if (trans % 2 == 0) {
            // max(buy today, don't buy today)
            int buy = -prices[i] + helper(i + 1, trans + 1, prices, k, dp);
            int notBuy = helper(i + 1, trans, prices, k, dp);
            return dp[i][trans] = Math.max(buy, notBuy);
        }
        // If transaction number is odd, it's a "sell" turn.
        else {
            // max(sell today, don't sell today)
            int sell = prices[i] + helper(i + 1, trans + 1, prices, k, dp);
            int notSell = helper(i + 1, trans, prices, k, dp);
            return dp[i][trans] = Math.max(sell, notSell);
        }
    }

    public int maxProfit(int k, int[] prices) {
        int n = prices.length;
        // DP state: [day][transaction_number]
        int[][] dp = new int[n][k * 2 + 1];
        for (int i = 0; i < n; i++) Arrays.fill(dp[i], -1);
        
        // Start on day 0, with transaction number 0 (first buy).
        return helper(0, 0, prices, k, dp);
    }

    // Time Complexity: O(N * K)
    // - Where N is the number of days and K is the transaction limit.
    // - Each state (N * 2K) is computed once.

    // Space Complexity: O(N * K)
    // - For the 2D DP table and the recursion stack.
}
````
## tabulation
````java
class Solution {
    public int maxProfit(int k, int[] prices) {
         int n=prices.length;
        int[][] dp=new int[n+1][k*2+1];
        for(int i=0;i<n+1;i++)dp[i][k*2]=0;
        for(int j=0;j<k*2+1;j++)dp[n][j]=0;
        for(int i=n-1;i>=0;i--){
            for(int j=0;j<k*2;j++){
                if(j%2==0){
            int buy=-prices[i]+dp[i+1][j+1];
            int notBuy=dp[i+1][j];
         dp[i][j]=Math.max(buy,notBuy);
        }
        else {
            int sell=prices[i]+dp[i+1][j+1];
            int notSell=dp[i+1][j];
             dp[i][j]=Math.max(sell,notSell);
        }
            }
        }
        return dp[0][0];
    }
}
````
## space optimised
````java
class Solution {
    public int maxProfit(int k, int[] prices) {
         int n=prices.length;
        int[] ahead=new int[k*2+1];
        ahead[k*2]=0;
        for(int j=0;j<k*2+1;j++)ahead[j]=0;
        for(int i=n-1;i>=0;i--){
            int[] cur=new int[k*2+1];
            for(int j=0;j<k*2;j++){
                if(j%2==0){
            int buy=-prices[i]+ahead[j+1];
            int notBuy=ahead[j];
            cur[j]=Math.max(buy,notBuy);
        }
        else {
            int sell=prices[i]+ahead[j+1];
            int notSell=ahead[j];
             cur[j]=Math.max(sell,notSell);
        }
            }
            ahead=cur;
        }
        return ahead[0];
    }
}
````
