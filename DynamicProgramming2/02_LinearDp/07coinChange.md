````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 322 - Coin Change
 * ----------------------------------------------------------------------
 * LOGIC (Top-Down Dynamic Programming / Memoization):
 * 1. The goal is to find the minimum number of coins needed to make up 
 * a specific 'amount'. We have an infinite supply of each coin type.
 * 2. At any given 'amount', we try picking every available coin.
 * 3. State Transition: 
 * For every coin 'x' in coins: 
 * current_coins = 1 + helper(amount - x)
 * We want the absolute minimum of these choices.
 * 4. Base Cases:
 * - If amount == 0: It takes 0 coins to make an amount of 0.
 * - If amount < 0: We overshot the target. Return a very large number 
 * (like 1e9) to mark this path as invalid. (Using 1e9 instead of 
 * Integer.MAX_VALUE is a smart choice to prevent integer overflow 
 * when we do '1 + helper(...)').
 * ----------------------------------------------------------------------
 */

import java.util.Arrays;

class Solution {
    
    public int helper(int amount, int[] coins, int[] dp) {
        // Base case 1: Target reached perfectly
        if (amount == 0) return 0;
        
        // Base case 2: Target overshot, invalid path
        if (amount < 0) return (int)(1e9);

        // Memoization check: Return previously computed minimum coins for this amount
        if (dp[amount] != -1) return dp[amount];
        
        // Initialize to our safe "infinity" value
        int minCoins = (int)(1e9);
        
        // Explore taking 1 of EVERY available coin denomination
        for (int x : coins) {
            // 1 represents the coin we just took. 
            // helper() finds the optimal way to make the remaining amount.
            int res = 1 + helper(amount - x, coins, dp);
            
            // Keep track of the best (minimum) route
            minCoins = Math.min(minCoins, res);
        }
        
        // Cache the result before returning
        return dp[amount] = minCoins;
    }
    
    public int coinChange(int[] coins, int amount) {
        // dp array tracks the minimum coins needed for every amount from 0 to 'amount'
        int[] dp = new int[amount + 1];
        Arrays.fill(dp, -1);
        
        int ans = helper(amount, coins, dp);
        
        // If the answer is still >= 1e9, it means no combination of coins 
        // could form the exact amount, so we return -1 per the problem description.
        return (ans >= (int)1e9) ? -1 : ans;
    }
}

/*
 * COMPLEXITY:
 * Time: O(amount * N) where N is the number of coin denominations. 
 * We solve for 'amount' states, and for each state, we loop through all N coins.
 * Space: O(amount) for the DP array and the maximum depth of the recursion call stack.
 */
````