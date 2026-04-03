````java
/*
 * ----------------------------------------------------------------------
 * PROBLEM: LeetCode 198 - House Robber
 * ----------------------------------------------------------------------
 * LOGIC (Top-Down Dynamic Programming / Memoization):
 * 1. The goal is to maximize the total amount of money robbed without 
 * alerting the police (meaning you cannot rob two adjacent houses).
 * 2. At any given house (index 'idx'), you have a binary decision:
 * - Choice A (Take): Rob this house. You get `nums[idx]`, but you MUST 
 * skip the next house. Your next valid decision is at `idx + 2`.
 * - Choice B (Not Take): Skip this house. Your next valid decision 
 * starts immediately at `idx + 1`.
 * 3. We use a recursion tree to explore all valid combinations. To avoid 
 * the massive redundancy of pure recursion (which would recalculate 
 * subproblems and result in O(2^N) time), we cache the maximum possible 
 * money from each index in our 'dp' array.
 * ----------------------------------------------------------------------
 */

import java.util.Arrays;

class Solution {
    public int helper(int idx, int[] nums, int[] dp) {
        // Base case: If we've reached or passed the end of the street, there are no more houses
        if (idx >= nums.length) return 0;
        
        // Memoization check: If we've already calculated the max haul from this index, return it
        if (dp[idx] != -1) return dp[idx];
        
        // Choice 1: Do NOT rob the current house, move to the immediate next house
        int notTake = helper(idx + 1, nums, dp);
        
        // Choice 2: DO rob the current house, add its value, and skip the adjacent house
        int take = nums[idx] + helper(idx + 2, nums, dp);
        
        // The maximum we can rob from this point onward is the better of our two choices.
        // Cache this result before returning it up the call stack.
        return dp[idx] = Math.max(take, notTake);
    }
    
    public int rob(int[] nums) {
        // Initialize the DP array with -1 (representing uncalculated states).
        // Length is nums.length + 1 to safely handle array bounds.
        int[] dp = new int[nums.length + 1];
        Arrays.fill(dp, -1);
        
        // Start the recursive robbery plan from the very first house (index 0)
        return helper(0, nums, dp);
    }
}

/*
 * COMPLEXITY:
 * Time: O(N) where N is the number of houses. Because of memoization, 
 * we only calculate the max for each index exactly once.
 * Space: O(N) for the 'dp' array plus O(N) for the maximum depth of the recursion call stack.
 */
````